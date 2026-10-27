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
desert-drift.tsx
import { cn } from "@/lib/utils";

export const Component = () => {
  return (
    <div className={cn("w-full h-screen flex items-center justify-center bg-black")}>
      <div className="w-full h-full max-w-5xl rounded-xl overflow-hidden border border-gray-700 shadow-2xl">
        <iframe
          src="https://my.spline.design/untitled-k1KQe1bIq5W7lZvLark2ZzGe/"
          className="w-full h-full"
          frameBorder="0"
          allowFullScreen
        />
      </div>
    </div>
  );
};


demo.tsx
import { Component } from "@/components/ui/desert-drift";

export default function DemoOne() {
  return <Component />;
}

```


![[Screenshot 2026-10-21 at 1.26.22 AM.png]]


https://21st.dev/community/components/s/hero?preview=%2F%40patrickthomasdunn1988%2Fcomponents%2Fdesert-drift