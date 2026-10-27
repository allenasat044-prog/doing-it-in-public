[[ORBIT-EO ideas]]
You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure  
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles. 
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
sonar-grid.tsx
"use client"

import * as React from "react"
import { cn } from "@/lib/utils"

export interface SonarGridProps extends React.ComponentProps<"div"> {
  /** Distance between dots in CSS pixels. */
  spacing?: number
  /** Dot radius at rest, in CSS pixels. */
  dotRadius?: number
  /** Resting dot opacity (0–1). Dots on a wavefront go to 1. */
  baseOpacity?: number
  /** Any CSS color. Defaults to the theme's primary color, so it adapts to light/dark and brand themes. */
  color?: string
  /** Seconds between ambient pings. Set 0 to disable them. */
  pingEvery?: number
  /** Wavefront speed in CSS pixels per second. */
  speed?: number
  /** Thickness of the wavefront in CSS pixels. */
  ringWidth?: number
  /** How much a dot grows at the wave peak (0 = no growth, 2 = triple size). */
  amplitude?: number
  /** Emit a ping where the user taps or clicks. */
  interactive?: boolean
  /** Maximum simultaneous rings. Older rings are dropped first. */
  maxRings?: number
  /** Start with one ring already mid-expansion so the very first frame shows the idea. */
  seedPing?: boolean
  /** Where ambient pings (and the seed ping) may spawn, as fractions of width/height: [x0, y0, x1, y1]. */
  pingArea?: [number, number, number, number]
}

interface Ring {
  x: number
  y: number
  born: number
}

const MAX_DPR = 2
const TAU = Math.PI * 2

/**
 * SonarGrid — a decorative dot field that answers taps with expanding rings.
 * Canvas-based and theme-aware (it reads the resolved `text-primary` color), it idles
 * when no ring is alive, pauses off-screen and in hidden tabs, and renders a still grid
 * under `prefers-reduced-motion`. Children render on top of the field.
 */
export function SonarGrid({
  spacing = 26,
  dotRadius = 1.4,
  baseOpacity = 0.28,
  color,
  pingEvery = 2.4,
  speed = 260,
  ringWidth = 90,
  amplitude = 2.2,
  interactive = true,
  maxRings = 6,
  seedPing = true,
  pingArea = [0.15, 0.2, 0.85, 0.8],
  className,
  children,
  ref,
  ...rest
}: SonarGridProps) {
  const hostRef = React.useRef<HTMLDivElement | null>(null)
  const canvasRef = React.useRef<HTMLCanvasElement>(null)
  const ringsRef = React.useRef<Ring[]>([])
  const refreshRef = React.useRef<() => void>(() => {})

  // The render loop reads props through this ref so knob changes apply live without restarting it.
  const opts = React.useRef({ spacing, dotRadius, baseOpacity, pingEvery, speed, ringWidth, amplitude, interactive, maxRings, seedPing, pingArea })
  opts.current = { spacing, dotRadius, baseOpacity, pingEvery, speed, ringWidth, amplitude, interactive, maxRings, seedPing, pingArea }

  const setHost = React.useCallback(
    (node: HTMLDivElement | null) => {
      hostRef.current = node
      if (typeof ref === "function") ref(node)
      else if (ref) ref.current = node
    },
    [ref]
  )

  React.useEffect(() => {
    const host = hostRef.current
    const canvas = canvasRef.current
    if (!host || !canvas) return
    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const reduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)")
    let width = 0
    let height = 0
    let raf = 0
    let timer = 0
    let visible = true
    let seeded = false
    let stroke = ""
    let nextPing = performance.now() + opts.current.pingEvery * 1000

    const readColor = () => {
      stroke = getComputedStyle(canvas).color
    }

    const addRing = (x: number, y: number, born: number) => {
      readColor()
      const rings = ringsRef.current
      rings.push({ x, y, born })
      while (rings.length > opts.current.maxRings) rings.shift()
    }

    const draw = (now: number) => {
      const o = opts.current
      const lifetime = (Math.hypot(width, height) + o.ringWidth) / o.speed // seconds until a ring leaves the canvas
      ringsRef.current = ringsRef.current.filter((r) => (now - r.born) / 1000 < lifetime)
      const live = ringsRef.current.map((r) => {
        const age = (now - r.born) / 1000
        const radius = age * o.speed
        return { x: r.x, y: r.y, radius, reach: radius + o.ringWidth, fade: 1 - age / lifetime }
      })

      ctx.clearRect(0, 0, width, height)
      ctx.fillStyle = stroke

      const cols = Math.ceil(width / o.spacing) + 1
      const rows = Math.ceil(height / o.spacing) + 1
      const offsetX = (width - (cols - 1) * o.spacing) / 2
      const offsetY = (height - (rows - 1) * o.spacing) / 2

      // Pass 1: every resting dot in a single path and a single fill.
      const hot: number[] = []
      ctx.globalAlpha = o.baseOpacity
      ctx.beginPath()
      for (let i = 0; i < cols; i++) {
        const cx = offsetX + i * o.spacing
        for (let j = 0; j < rows; j++) {
          const cy = offsetY + j * o.spacing
          let energy = 0
          for (const r of live) {
            if (Math.abs(cx - r.x) > r.reach || Math.abs(cy - r.y) > r.reach) continue
            const dist = Math.abs(Math.hypot(cx - r.x, cy - r.y) - r.radius)
            if (dist >= o.ringWidth) continue
            const t = 1 - dist / o.ringWidth
            const k = t * t * (3 - 2 * t) * r.fade // smoothstep, fading with age
            if (k > energy) energy = k
          }
          if (energy < 0.01) {
            ctx.moveTo(cx + o.dotRadius, cy)
            ctx.arc(cx, cy, o.dotRadius, 0, TAU)
          } else {
            hot.push(cx, cy, energy)
          }
        }
      }
      ctx.fill()

      // Pass 2: only the dots on a wavefront get their own alpha and radius.
      for (let k = 0; k < hot.length; k += 3) {
        const energy = hot[k + 2] ?? 0
        ctx.globalAlpha = o.baseOpacity + (1 - o.baseOpacity) * energy
        ctx.beginPath()
        ctx.arc(hot[k] ?? 0, hot[k + 1] ?? 0, o.dotRadius * (1 + o.amplitude * energy), 0, TAU)
        ctx.fill()
      }
      ctx.globalAlpha = 1
    }

    const resize = () => {
      const rect = host.getBoundingClientRect()
      width = Math.max(1, Math.round(rect.width))
      height = Math.max(1, Math.round(rect.height))
      const dpr = Math.min(window.devicePixelRatio || 1, MAX_DPR)
      canvas.width = Math.round(width * dpr)
      canvas.height = Math.round(height * dpr)
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0)
      if (!seeded) {
        // One ring already mid-expansion inside the ping area, so the first paint (and the cover) shows the idea.
        seeded = true
        const [x0, y0, x1, y1] = opts.current.pingArea
        if (opts.current.seedPing && !reduceMotion.matches)
          addRing(width * (x0 + (x1 - x0) * 0.68), height * (y0 + (y1 - y0) * 0.34), performance.now() - 500)
      }
      draw(performance.now())
    }

    const scheduleIdle = (delay: number) => {
      window.clearTimeout(timer)
      timer = window.setTimeout(() => tick(performance.now()), Math.max(16, delay))
    }

    const tick = (now: number) => {
      raf = 0
      if (!visible || document.hidden) return
      if (reduceMotion.matches) {
        ringsRef.current = []
        draw(now)
        return
      }
      const o = opts.current
      if (o.pingEvery > 0 && now >= nextPing) {
        const [x0, y0, x1, y1] = o.pingArea
        addRing(width * (x0 + Math.random() * (x1 - x0)), height * (y0 + Math.random() * (y1 - y0)), now)
        nextPing = now + o.pingEvery * 1000
      }
      draw(now)
      if (ringsRef.current.length > 0) raf = requestAnimationFrame(tick)
      else if (o.pingEvery > 0) scheduleIdle(nextPing - now)
    }

    const wake = () => {
      if (!raf) {
        window.clearTimeout(timer)
        raf = requestAnimationFrame(tick)
      }
    }

    refreshRef.current = () => {
      readColor()
      nextPing = Math.min(nextPing, performance.now() + opts.current.pingEvery * 1000)
      wake()
    }

    const onDown = (e: PointerEvent) => {
      if (!opts.current.interactive || reduceMotion.matches) return
      const rect = host.getBoundingClientRect()
      addRing(e.clientX - rect.left, e.clientY - rect.top, performance.now())
      wake()
    }
    const onVisibility = () => {
      if (!document.hidden) wake()
    }

    const ro = new ResizeObserver(resize)
    const io = new IntersectionObserver(
      ([entry]) => {
        visible = entry?.isIntersecting ?? true
        if (visible) wake()
      },
      { threshold: 0 }
    )
    const mo = new MutationObserver(() => refreshRef.current())

    readColor()
    resize()
    ro.observe(host)
    io.observe(host)
    mo.observe(document.documentElement, { attributes: true, attributeFilter: ["class", "style", "data-theme"] })
    host.addEventListener("pointerdown", onDown)
    document.addEventListener("visibilitychange", onVisibility)
    reduceMotion.addEventListener("change", wake)
    wake()

    return () => {
      ro.disconnect()
      io.disconnect()
      mo.disconnect()
      host.removeEventListener("pointerdown", onDown)
      document.removeEventListener("visibilitychange", onVisibility)
      reduceMotion.removeEventListener("change", wake)
      cancelAnimationFrame(raf)
      window.clearTimeout(timer)
      refreshRef.current = () => {}
    }
  }, [])

  // Prop changes while the loop is asleep still repaint immediately.
  React.useEffect(() => {
    refreshRef.current()
  }, [spacing, dotRadius, baseOpacity, color, pingEvery, speed, ringWidth, amplitude, interactive, maxRings, pingArea])

  return (
    <div
      ref={setHost}
      data-slot="sonar-grid"
      className={cn("relative isolate overflow-hidden", interactive && "cursor-crosshair", className)}
      {...rest}
    >
      <canvas
        ref={canvasRef}
        aria-hidden="true"
        className="text-primary pointer-events-none absolute inset-0 -z-10 size-full"
        style={color ? { color } : undefined}
      />
      {children}
    </div>
  )
}

export default SonarGrid


demo.tsx
"use client"

import { ArrowRight } from "lucide-react"
import { motion, useReducedMotion } from "motion/react"
// Relative import on purpose: the 21st CLI rewrites it to the published path (`@/components/ui/<slug>`)
// and to a temporary path during `21st render`. An absolute "@/…" import breaks the render build.
import { SonarGrid } from "@/components/ui/sonar-grid"

// Module-level `settings` + a default export that accepts them as props gives this
// demo a live controls panel on 21st.dev. Signature knobs first, then look, then copy.
const settings = {
  ringWidth: 90,
  speed: 260,
  amplitude: 2.2,
  pingEvery: 2.4,
  interactive: true,
  spacing: 26,
  baseOpacity: 0.28,
  useThemeColor: true,
  color: "#6366f1",
  eyebrow: "Now in public beta",
  headline: "Signals, not noise.",
  subline: "Every ping is a real event from your infrastructure. Tap anywhere to send one.",
}

export default function Demo(props: Partial<typeof settings>) {
  const s = { ...settings, ...props }
  const reduce = useReducedMotion()
  const enter = (delay: number) =>
    reduce
      ? {}
      : {
          initial: { opacity: 0, y: 14, filter: "blur(6px)" },
          animate: { opacity: 1, y: 0, filter: "blur(0px)" },
          transition: { duration: 0.6, delay, ease: [0.22, 1, 0.36, 1] as const },
        }

  return (
    <SonarGrid
      id="sonar-grid-demo"
      ringWidth={s.ringWidth}
      speed={s.speed}
      amplitude={s.amplitude}
      pingEvery={s.pingEvery}
      interactive={s.interactive}
      spacing={s.spacing}
      baseOpacity={s.baseOpacity}
      color={s.useThemeColor ? undefined : s.color}
      pingArea={[0.22, 0.18, 0.78, 0.82]}
      className="bg-background flex min-h-[max(560px,100svh)] w-full flex-col"
    >
      {/* A soft wash behind the copy keeps it legible while rings pass underneath. */}
      <div
        aria-hidden="true"
        className="pointer-events-none absolute inset-0 -z-10 bg-[radial-gradient(ellipse_34%_30%_at_50%_50%,var(--color-background)_0%,transparent_100%)]"
      />
      {/*
        Centered on purpose. 21st captures covers at 1280×960 and applies a content-aware crop that
        follows dark pixels: a left-aligned headline with rings on the right got cropped. A balanced
        composition keeps headline and rings together in every capture.
      */}
      <div className="mx-auto flex w-full max-w-6xl flex-1 flex-col items-center justify-center px-8 py-24 text-center">
        <div className="flex max-w-2xl flex-col items-center">
          <motion.p
            {...enter(0)}
            className="text-muted-foreground border-border mb-5 inline-flex items-center gap-2 rounded-full border px-3 py-1 text-xs font-medium"
          >
            <span aria-hidden="true" className="bg-primary size-1.5 rounded-full" />
            {s.eyebrow}
          </motion.p>
          <motion.h1
            {...enter(0.08)}
            className="text-foreground text-5xl font-semibold tracking-tight text-balance sm:text-6xl md:text-7xl"
          >
            {s.headline}
          </motion.h1>
          <motion.p {...enter(0.16)} className="text-muted-foreground mt-6 max-w-xl text-base text-pretty sm:text-lg">
            {s.subline}
          </motion.p>
          <motion.div {...enter(0.24)} className="mt-9 flex flex-wrap items-center justify-center gap-3">
            <button
              type="button"
              id="cta-primary"
              data-slot="cta-primary"
              className="group bg-primary text-primary-foreground focus-visible:ring-ring/50 inline-flex h-11 cursor-pointer items-center gap-2 rounded-full px-6 text-sm font-medium shadow-sm transition-[transform,box-shadow] duration-200 outline-none hover:shadow-md focus-visible:ring-[3px] active:scale-[0.98]"
            >
              Start listening
              <ArrowRight
                aria-hidden="true"
                className="size-4 transition-transform duration-200 group-hover:translate-x-0.5"
              />
            </button>
            <a
              href="#docs"
              data-slot="cta-secondary"
              className="bg-background/70 text-foreground border-border hover:bg-accent focus-visible:ring-ring/50 inline-flex h-11 cursor-pointer items-center rounded-full border px-6 text-sm font-medium backdrop-blur transition-[background-color,transform] duration-200 outline-none focus-visible:ring-[3px] active:scale-[0.98]"
            >
              Read the docs
            </a>
          </motion.div>
        </div>
      </div>
    </SonarGrid>
  )
}

```

Install NPM dependencies:
```bash
motion
```


![[Screenshot 2026-10-25 at 2.51.27 PM.png]]

https://21st.dev/community/components/s/background?preview=%2F%40n1m4mz%2Fcomponents%2Fsonar-grid