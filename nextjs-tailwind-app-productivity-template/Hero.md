import { Button } from "@/components/ui/button";
import Image from "next/image";

export const Hero = () => {
  return (
    <section className="relative overflow-hidden py-24 sm:py-32">

      {/* glow effects */}
      <div className="absolute -top-40 left-1/2 h-125 w-125 -translate-x-1/2 rounded-full bg-primary/20 blur-[120px]" />
      <div className="absolute bottom-0 right-0 h-100 w-100 rounded-full bg-accent/20 blur-[120px]" />

      <div className="container mx-auto px-6">
        <div className="mx-auto max-w-4xl text-center">
          {/* tagline */}
          <div className="mb-6 flex justify-center">
            <span className="rounded-full border border-border bg-muted/40 px-4 py-1 text-xs text-muted-foreground backdrop-blur">
              The future of productivity
            </span>
          </div>

          {/* headline */}
          <h1 className="text-4xl font-bold tracking-tight sm:text-6xl">
            Build workflows that
            <span className="bg-linear-to-r from-primary to-accent bg-clip-text text-transparent">
              {" "}
              actually save time
            </span>
          </h1>

          {/* subheadline */}
          <p className="mt-6 text-lg text-muted-foreground sm:text-xl">
            Organize tasks, automate repetitive work, and focus on what matters.
            A modern productivity platform designed for teams and creators.
          </p>

          {/* CTA */}
          <div className="mt-10 flex flex-col items-center gap-4 sm:flex-row sm:justify-center">
            <Button size="lg">Start for free</Button>

            <Button size="lg" variant="outline">
              Live demo
            </Button>
          </div>
        </div>

        {/* product preview */}
        <div className="relative w-full aspect-video ">
            <Image
              src="/images/image-dashboard.png"
              alt="FocusKit dashboard"
              fill
              className="object-cover object-top bg-linear-to-b"
              priority
            />
          </div>
      </div>
    </section>
  );
}
