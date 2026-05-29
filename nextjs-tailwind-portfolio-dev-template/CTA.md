"use client"

import Link from "next/link"
import { motion } from "motion/react"
import { personalConfig, ctaLink, socialLinks } from "@/lib/config"
import {fadeUp} from "@/lib/helpers/fadeUp"

// ─── Animated grid background ─────────────────────────────────────────────────

function GridBackground() {
  return (
    <div aria-hidden className="pointer-events-none absolute inset-0 overflow-hidden">
      {/* Grid */}
      <div
        className="absolute inset-0 opacity-[0.03]"
        style={{
          backgroundImage:
            "linear-gradient(hsl(var(--foreground)) 1px, transparent 1px), linear-gradient(90deg, hsl(var(--foreground)) 1px, transparent 1px)",
          backgroundSize: "48px 48px",
        }}
      />

      {/* Center orb grande */}
      <div className="absolute left-1/2 top-1/2 h-150 w-150 -translate-x-1/2 -translate-y-1/2 rounded-full bg-primary/8 blur-[140px]" />

      {/* Accent orb offset */}
      <div className="absolute -bottom-20 left-1/2 ml-40 h-75 w-75 rounded-full bg-accent/6 blur-[100px]" />

      {/* Vignette edges */}
      <div className="absolute inset-0 bg-linear-to-b from-background via-transparent to-background" />
      <div className="absolute inset-0 bg-linear-to-r from-background via-transparent to-background" />
    </div>
  )
}

// ─── CTA ──────────────────────────────────────────────────────────────────────

export function CTA() {
  return (
    <section className="relative overflow-hidden px-6 py-32 lg:px-8 lg:py-40">

      {/* Top border */}
      <div
        aria-hidden
        className="pointer-events-none absolute inset-x-0 top-0 h-px bg-linear-to-r from-transparent via-border to-transparent"
      />

      <GridBackground />

      <div className="relative mx-auto max-w-4xl text-center">

        {/* Eyebrow */}
        <motion.div {...fadeUp(0)} className="mb-6 flex justify-center">
          <span className="inline-flex items-center gap-2 rounded-full border border-primary/20 bg-primary/8 px-4 py-1.5 text-xs font-medium tracking-wide text-primary">
            <span className="relative flex h-1.5 w-1.5">
              <span className="absolute inline-flex h-full w-full animate-ping rounded-full bg-primary opacity-75" />
              <span className="relative inline-flex h-1.5 w-1.5 rounded-full bg-primary" />
            </span>
            {personalConfig.availability} — taking new projects
          </span>
        </motion.div>

        {/* Heading */}
        <motion.h2
          {...fadeUp(0.08)}
          className="mb-6 text-4xl font-semibold leading-[1.08] tracking-tight text-foreground sm:text-5xl md:text-6xl"
        >
          Let's build something{" "}
          <span className="bg-linear-to-r from-primary via-accent to-primary bg-clip-text text-transparent bg-size-[200%_auto] animate-[gradient_4s_linear_infinite]">
            remarkable.
          </span>
        </motion.h2>

        {/* Subtext */}
        <motion.p
          {...fadeUp(0.14)}
          className="mx-auto mb-12 max-w-xl text-base leading-relaxed text-muted-foreground sm:text-lg"
        >
          Have a project in mind or just want to explore what we could build
          together? I'd love to hear about it.
        </motion.p>

        {/* CTAs */}
        <motion.div
          {...fadeUp(0.2)}
          className="mb-16 flex flex-col items-center justify-center gap-3 sm:flex-row"
        >
          {/* Primary */}
          <Link
            href={ctaLink.href}
            className="group relative inline-flex h-12 items-center overflow-hidden rounded-full bg-primary px-8 text-sm font-semibold text-primary-foreground shadow-[0_0_0_0_hsl(var(--primary)/40%)] transition-all duration-300 hover:shadow-[0_0_32px_hsl(var(--primary)/40%)]"
          >
            <span className="absolute inset-0 -translate-x-full bg-linear-to-r from-transparent via-white/15 to-transparent transition-transform duration-500 group-hover:translate-x-full" />
            <span className="relative flex items-center gap-2">
              Start a project
              <svg className="h-4 w-4 transition-transform duration-300 group-hover:translate-x-1" viewBox="0 0 16 16" fill="none">
                <path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" />
              </svg>
            </span>
          </Link>

          {/* Secondary */}
          <a
            href={`mailto:${personalConfig.email}`}
            className="inline-flex h-12 items-center gap-2 rounded-full border border-border px-8 text-sm font-medium text-foreground transition-all duration-200 hover:border-primary/40 hover:bg-primary/5"
          >
            <svg className="h-4 w-4 text-muted-foreground" viewBox="0 0 16 16" fill="none" stroke="currentColor" strokeWidth="1.5">
              <path d="M2 4h12c.6 0 1 .4 1 1v7c0 .6-.4 1-1 1H2c-.6 0-1-.4-1-1V5c0-.6.4-1 1-1z" strokeLinecap="round" strokeLinejoin="round" />
              <path d="M15 4.5L8 9.5 1 4.5" strokeLinecap="round" strokeLinejoin="round" />
            </svg>
            Send an email
          </a>
        </motion.div>

        {/* Divider */}
        <motion.div
          {...fadeUp(0.26)}
          className="mx-auto mb-10 h-px w-24 bg-linear-to-r from-transparent via-border to-transparent"
        />

        {/* Social links */}
        <motion.div
          {...fadeUp(0.3)}
          className="flex items-center justify-center gap-6"
        >
          {socialLinks.map((s) => (
            <a
              key={s.id}
              href={s.url}
              target={s.id !== "email" ? "_blank" : undefined}
              rel={s.id !== "email" ? "noopener noreferrer" : undefined}
              className="group flex flex-col items-center gap-1.5 text-muted-foreground/40 transition-colors duration-200 hover:text-foreground"
            >
              <span className="text-xs font-medium tracking-wide">
                {s.label}
              </span>
              <span className="h-px w-0 bg-primary transition-all duration-300 group-hover:w-full" />
            </a>
          ))}
        </motion.div>

      </div>

      {/* Decorative corner marks */}
      {(["top-6 left-6", "top-6 right-6", "bottom-6 left-6", "bottom-6 right-6"] as const).map((pos) => (
        <div
          key={pos}
          aria-hidden
          className={`pointer-events-none absolute ${pos} h-4 w-4 opacity-20`}
        >
          <div className="absolute left-0 top-0 h-full w-px bg-primary" />
          <div className="absolute left-0 top-0 h-px w-full bg-primary" />
        </div>
      ))}

    </section>
  )
}