"use client"

import { useState } from "react"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { cn } from "@/lib/utils"
import { settingsConfig, navItemsSettings as navItems, Section } from "@/lib/dashboard-config"


function Toggle({ value, onChange }: { value: boolean; onChange: () => void }) {
  return (
    <button
      onClick={onChange}
      className={cn(
        "w-8 h-4 rounded-full relative transition-colors shrink-0",
        value ? "bg-primary" : "bg-border"
      )}
    >
      <div className={cn(
        "absolute top-0.5 w-3 h-3 rounded-full bg-white transition-all",
        value ? "right-0.5" : "left-0.5"
      )} />
    </button>
  )
}

function SettingRow({ label, description, children }: {
  label: string
  description?: string
  children: React.ReactNode
}) {
  return (
    <div className="flex items-center justify-between gap-4 py-4 border-b border-border last:border-0">
      <div className="flex-1 min-w-0">
        <p className="text-sm text-foreground mb-0.5">{label}</p>
        {description && <p className="text-xs text-muted-foreground">{description}</p>}
      </div>
      {children}
    </div>
  )
}

function Stepper({ value, onChange, unit, min = 1, max = 60 }: {
  value: number; onChange: (v: number) => void
  unit?: string; min?: number; max?: number
}) {
  return (
    <div className="flex items-center gap-1.5 shrink-0">
      <button
        onClick={() => onChange(Math.max(min, value - 1))}
        className="w-6 h-6 rounded border border-border flex items-center justify-center text-muted-foreground hover:text-foreground transition-colors"
      >−</button>
      <span className="text-sm font-medium text-foreground w-10 text-center">
        {value}{unit}
      </span>
      <button
        onClick={() => onChange(Math.min(max, value + 1))}
        className="w-6 h-6 rounded border border-border flex items-center justify-center text-muted-foreground hover:text-foreground transition-colors"
      >+</button>
    </div>
  )
}

export default function SettingsPage() {
  const [section, setSection] = useState<Section>("profile")
  const [saved, setSaved]     = useState(false)

  // Profile
  const [profile, setProfile] = useState(settingsConfig.profile)

  // Preferences
  const [prefs, setPrefs] = useState(settingsConfig.preferences)

  // Pomodoro
  const [pomodoro, setPomodoro] = useState(settingsConfig.pomodoro)

  // Notifications
  const [notifs, setNotifs] = useState(settingsConfig.notifications)

  const handleSave = () => {
    setSaved(true)
    setTimeout(() => setSaved(false), 2000)
  }

  return (
    <div className="grid grid-cols-1 md:grid-cols-[200px_1fr] gap-3 items-start">

      {/* Nav */}
      <div className="bg-muted/20 rounded-xl p-2 flex flex-col gap-0.5">
        <p className="text-[10px] font-medium uppercase tracking-widest text-muted-foreground px-2.5 py-1.5 mb-1">
          Settings
        </p>
        {navItems.map((item) => {
          const Icon = item.icon
          return (
            <button
              key={item.id}
              onClick={() => setSection(item.id)}
              className={cn(
                "flex items-center gap-2.5 px-2.5 py-2 rounded-md text-xs transition-colors w-full text-left",
                section === item.id
                  ? "bg-primary/10 text-primary font-medium border border-primary/20"
                  : "text-muted-foreground hover:text-foreground hover:bg-muted/30"
              )}
            >
              <Icon className="w-3.5 h-3.5 shrink-0" />
              {item.label}
            </button>
          )
        })}
      </div>

      {/* Content */}
      <div className="bg-muted/20 rounded-xl overflow-hidden">

        {/* Section header */}
        <div className="px-6 py-4 border-b border-border">
          <p className="text-sm font-medium text-foreground capitalize">
            {navItems.find((n) => n.id === section)?.label}
          </p>
          <p className="text-xs text-muted-foreground mt-0.5">
            {section === "profile"       && "Update your personal information."}
            {section === "preferences"   && "Customize your dashboard experience."}
            {section === "pomodoro"      && "Configure your focus and break durations."}
            {section === "notifications" && "Control what alerts you receive."}
            {section === "account"       && "Manage your plan and account security."}
          </p>
        </div>

        <div className="px-6 py-2">

          {/* Profile */}
          {section === "profile" && (
            <div className="flex flex-col gap-5 py-4">
              <div className="flex items-center gap-4 pb-5 border-b border-border">
                <div className="w-14 h-14 rounded-full bg-green-400/10 border border-green-400/20 flex items-center justify-center text-lg font-medium text-green-400 shrink-0">
                  {profile.firstName[0]}{profile.lastName[0]}
                </div>
                <div>
                  <p className="text-sm font-medium text-foreground mb-1">
                    {profile.firstName} {profile.lastName}
                  </p>
                  <p className="text-xs text-muted-foreground mb-2">Pro plan</p>
                  <button className="text-xs px-3 py-1 border border-border rounded-md text-muted-foreground hover:text-foreground transition-colors">
                    Change avatar
                  </button>
                </div>
              </div>

              <div className="grid grid-cols-2 gap-3">
                <div className="flex flex-col gap-1.5">
                  <Label className="text-xs">First name</Label>
                  <Input value={profile.firstName} onChange={(e) => setProfile((p) => ({ ...p, firstName: e.target.value }))} className="h-8 text-xs bg-card" />
                </div>
                <div className="flex flex-col gap-1.5">
                  <Label className="text-xs">Last name</Label>
                  <Input value={profile.lastName} onChange={(e) => setProfile((p) => ({ ...p, lastName: e.target.value }))} className="h-8 text-xs bg-card" />
                </div>
              </div>

              <div className="flex flex-col gap-1.5">
                <Label className="text-xs">Email</Label>
                <Input value={profile.email} onChange={(e) => setProfile((p) => ({ ...p, email: e.target.value }))} className="h-8 text-xs bg-card" />
              </div>

              <div className="flex flex-col gap-1.5">
                <Label className="text-xs">Role</Label>
                <select
                  value={profile.role}
                  onChange={(e) => setProfile((p) => ({ ...p, role: e.target.value }))}
                  className="h-8 px-2.5 text-xs bg-card border border-border rounded-md text-foreground"
                >
                  <option>Student</option>
                  <option>Developer</option>
                  <option>Both</option>
                </select>
              </div>

              <div className="flex justify-end pt-2 border-t border-border">
                <button onClick={handleSave} className={cn("px-4 py-1.5 rounded-md text-xs font-medium transition-all", saved ? "bg-green-400/10 text-green-400 border border-green-400/20" : "bg-foreground text-background hover:opacity-90")}>
                  {saved ? "Saved ✓" : "Save changes"}
                </button>
              </div>
            </div>
          )}

          {/* Preferences */}
          {section === "preferences" && (
            <div className="py-1">
              {[
                { label: "Start of week",  key: "startOfWeek",  opts: ["Monday", "Sunday"] },
                { label: "Date format",    key: "dateFormat",   opts: ["MM/DD/YYYY", "DD/MM/YYYY", "YYYY-MM-DD"] },
                { label: "Default view",   key: "defaultView",  opts: ["Overview", "Tasks", "Pomodoro"] },
              ].map((item) => (
                <SettingRow key={item.key} label={item.label}>
                  <select
                    value={prefs[item.key as keyof typeof prefs] as string}
                    onChange={(e) => setPrefs((p) => ({ ...p, [item.key]: e.target.value }))}
                    className="h-8 px-2.5 text-xs bg-card border border-border rounded-md text-foreground"
                  >
                    {item.opts.map((o) => <option key={o}>{o}</option>)}
                  </select>
                </SettingRow>
              ))}
              {[
                { label: "Show completed tasks", key: "showCompleted", desc: "Keep done tasks visible in the list" },
                { label: "Compact mode",          key: "compactMode",   desc: "Reduce spacing for a denser layout" },
                { label: "Show task tags",         key: "showTags",      desc: "Display category badges on task cards" },
              ].map((item) => (
                <SettingRow key={item.key} label={item.label} description={item.desc}>
                  <Toggle
                    value={prefs[item.key as keyof typeof prefs] as boolean}
                    onChange={() => setPrefs((p) => ({ ...p, [item.key]: !p[item.key as keyof typeof p] }))}
                  />
                </SettingRow>
              ))}
            </div>
          )}

          {/* Pomodoro */}
          {section === "pomodoro" && (
            <div className="py-1">
              {[
                { label: "Focus duration",            key: "focus",    unit: "m", desc: "Minutes per focus session" },
                { label: "Short break",               key: "short",    unit: "m", desc: "Break between sessions" },
                { label: "Long break",                key: "long",     unit: "m", desc: "Break after N sessions" },
                { label: "Sessions before long break", key: "sessions", unit: "",  desc: "How many sessions before a long break", max: 8 },
              ].map((item) => (
                <SettingRow key={item.key} label={item.label} description={item.desc}>
                  <Stepper
                    value={pomodoro[item.key as keyof typeof pomodoro] as number}
                    onChange={(v) => setPomodoro((p) => ({ ...p, [item.key]: v }))}
                    unit={item.unit}
                    max={item.max ?? 60}
                  />
                </SettingRow>
              ))}
              {[
                { label: "Auto-start breaks", key: "autoBreak",  desc: "Automatically start break when focus ends" },
                { label: "Auto-start focus",  key: "autoFocus",  desc: "Automatically start next focus after break" },
                { label: "Sound alerts",      key: "soundAlerts", desc: "Play a sound when a session ends" },
              ].map((item) => (
                <SettingRow key={item.key} label={item.label} description={item.desc}>
                  <Toggle
                    value={pomodoro[item.key as keyof typeof pomodoro] as boolean}
                    onChange={() => setPomodoro((p) => ({ ...p, [item.key]: !p[item.key as keyof typeof p] }))}
                  />
                </SettingRow>
              ))}
            </div>
          )}

          {/* Notifications */}
          {section === "notifications" && (
            <div className="py-1">
              {notifs.map((notif, i) => (
                <SettingRow key={i} label={notif.label} description={notif.description}>
                  <Toggle
                    value={notif.enabled}
                    onChange={() => setNotifs((prev) =>
                      prev.map((n, idx) => idx === i ? { ...n, enabled: !n.enabled } : n)
                    )}
                  />
                </SettingRow>
              ))}
            </div>
          )}

          {/* Account */}
          {section === "account" && (
            <div className="flex flex-col gap-4 py-4">
              <div className="bg-card border border-primary/20 rounded-xl p-4 flex items-center justify-between gap-3">
                <div>
                  <span className="text-xs font-medium text-primary bg-primary/10 border border-primary/20 px-2 py-0.5 rounded-full inline-block mb-1.5">
                    Pro plan
                  </span>
                  <p className="text-xs text-muted-foreground">Renews Jan 15, 2025 · $9/mo</p>
                </div>
                <button className="text-xs px-3 py-1.5 border border-border rounded-md text-muted-foreground hover:text-foreground transition-colors whitespace-nowrap">
                  Manage plan
                </button>
              </div>

              <div className="border border-border rounded-xl overflow-hidden">
                <div className="px-5 py-3 border-b border-border">
                  <p className="text-[10px] font-medium uppercase tracking-widest text-muted-foreground mb-3">Security</p>
                  <SettingRow label="Password" description="Last changed 3 months ago">
                    <button className="text-xs px-3 py-1.5 border border-border rounded-md text-muted-foreground hover:text-foreground transition-colors">
                      Change
                    </button>
                  </SettingRow>
                </div>
                <div className="px-5 py-3">
                  <SettingRow label="Two-factor authentication" description="Add an extra layer of security">
                    <button className="text-xs px-3 py-1.5 border border-border rounded-md text-muted-foreground hover:text-foreground transition-colors">
                      Enable
                    </button>
                  </SettingRow>
                </div>
              </div>

              <div className="pt-2 border-t border-border">
                <p className="text-[10px] font-medium uppercase tracking-widest text-muted-foreground mb-3">
                  Danger zone
                </p>
                <div className="flex gap-2 flex-wrap">
                  <button className="text-xs px-3 py-1.5 border border-border rounded-md text-muted-foreground hover:text-foreground transition-colors">
                    Export data
                  </button>
                  <button className="text-xs px-3 py-1.5 border border-red-400/30 rounded-md text-red-400 hover:bg-red-400/5 transition-colors">
                    Delete account
                  </button>
                </div>
              </div>
            </div>
          )}

        </div>
      </div>
    </div>
  )
}