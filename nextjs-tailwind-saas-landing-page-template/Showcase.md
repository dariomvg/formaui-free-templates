"use client"

import { useState } from "react"
import Image from "next/image"
import { cn } from "@/lib/utils"
import {tabs} from "@/lib/config"


export function Showcase() {
  const [active, setActive] = useState(0)

  return (
    <section className="w-full py-24 px-4 md:px-6" id="showcase">
      <div className="max-w-6xl mx-auto">

        {/* Header */}
        <div className="text-center mb-10">
          <span className="text-xs font-medium uppercase tracking-widest text-primary border border-primary/20 bg-primary/10 px-3 py-1 rounded-full">
            Product
          </span>
          <h2 className="text-4xl font-semibold mt-4 mb-3 text-foreground">
            Everything in one place.
          </h2>
          <p className="text-muted-foreground text-base max-w-md mx-auto">
            No more switching tabs. NovaMind brings your metrics, reports and insights into a single dashboard.
          </p>
        </div>

        {/* Tabs */}
        <div className="flex justify-center mb-6">
          <div className="inline-flex bg-muted border border-border rounded-lg p-1 gap-0.5">
            {tabs.map((tab, i) => (
              <button
                key={tab.label}
                onClick={() => setActive(i)}
                className={cn(
                  "text-sm font-medium px-4 py-1.5 rounded-md transition-all",
                  active === i
                    ? "bg-background text-foreground border border-border shadow-sm"
                    : "text-muted-foreground hover:text-foreground"
                )}
              >
                {tab.label}
              </button>
            ))}
          </div>
        </div>

        {/* Main image of browser*/}
        <div className="border border-border rounded-xl overflow-hidden mb-px">

          {/* Line of browser */}
          <div className="bg-muted/30 border-b border-border px-4 py-2.5 flex items-center gap-2">
            <div className="flex gap-1.5">
              <span className="w-2.5 h-2.5 rounded-full bg-border" />
              <span className="w-2.5 h-2.5 rounded-full bg-border" />
              <span className="w-2.5 h-2.5 rounded-full bg-border" />
            </div>
            <div className="flex-1 bg-background border border-border rounded px-3 py-1 mx-2">
              <span className="text-xs text-muted-foreground">app.novamind.io/dashboard</span>
            </div>
          </div>

          {/* Screenshot */}
          <div className="relative w-full aspect-video bg-muted/20">
            <Image
              src={tabs[active].img}
              alt={`${tabs[active].label} dashboard`}
              fill
              className="object-cover object-top"
            />
          </div>
        </div>

        {/* Thumbnails */}
        <div className="grid grid-cols-3 divide-x divide-border border border-border rounded-xl overflow-hidden mt-px">
          {tabs.map((tab, i) => {
            const Icon = tab.icon
            return (
              <button
                key={tab.label}
                onClick={() => setActive(i)}
                className={cn(
                  "flex items-center gap-3 p-5 text-left transition-colors",
                  active === i
                    ? "bg-muted/30"
                    : "bg-card hover:bg-muted/20"
                )}
              >
                <div className={cn(
                  "w-12 h-9 rounded border flex items-center justify-center shrink-0 transition-colors",
                  active === i
                    ? "bg-primary/10 border-primary/20"
                    : "bg-muted/30 border-border"
                )}>
                  <Icon className={cn(
                    "w-4 h-4 transition-colors",
                    active === i ? "text-primary" : "text-muted-foreground"
                  )} />
                </div>
                <div>
                  <p className="text-sm font-medium text-foreground">{tab.label}</p>
                  <p className="text-xs text-muted-foreground">{tab.description}</p>
                </div>
              </button>
            )
          })}
        </div>
      </div>
    </section>
  )
}
