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
border-beam-search.tsx
import { BorderBeam } from "border-beam";
import type {
  BorderBeamProps,
  BorderBeamSize,
  BorderBeamTheme,
  BorderBeamColorVariant,
} from "border-beam";

export type {
  BorderBeamProps,
  BorderBeamSize,
  BorderBeamTheme,
  BorderBeamColorVariant,
};

export { BorderBeam };
export default BorderBeam;


demo.tsx
import { BorderBeam } from "@/components/ui/border-beam-search";

function SearchIcon() {
  return (
    <svg aria-hidden="true" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" style={{ opacity: 0.4, flexShrink: 0 }}>
      <circle cx="11" cy="11" r="8" />
      <line x1="21" y1="21" x2="16.65" y2="16.65" />
    </svg>
  );
}

function SearchBar() {
  return (
    <div style={{ width: 366, maxWidth: "100%", height: 42, borderRadius: 64, overflow: "hidden", position: "relative", fontFamily: "system-ui, -apple-system, sans-serif" }}>
      <div
        style={{
          position: "absolute",
          inset: 0,
          borderRadius: 20,
          background: "#1d1d1d",
          boxShadow: "inset 0 0 0 1px rgba(44,47,54,0.52), inset 0 0 50px 0 rgba(255,255,255,0.02)",
          display: "flex",
          alignItems: "center",
          padding: "0 13px",
          gap: 10,
          color: "#565656",
        }}
      >
        <SearchIcon />
        <span style={{ fontSize: 15, lineHeight: "18px", color: "#565656" }}>Search</span>
      </div>
    </div>
  );
}

export default function Default() {
  return (
    <div
      style={{
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
        width: 600,
        maxWidth: "100%",
        minHeight: 360,
        margin: "0 auto",
        background: "#0d0d0f",
        borderRadius: 24,
      }}
    >
      <BorderBeam size="line" colorVariant="colorful" duration={3.1} borderRadius={20}>
        <SearchBar />
      </BorderBeam>
    </div>
  );
}

```

Install NPM dependencies:
```bash
border-beam
```
![[Screenshot 2026-10-18 at 7.55.05 PM.png]]


https://21st.dev/community/components/s/search?preview=%2F%40larsen66%2Fcomponents%2Fborder-beam-search