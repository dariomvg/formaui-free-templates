"use client";

import { useState, useEffect, useRef, useCallback } from "react";
import { RotateCcw, RotateCw, Check } from "lucide-react";
import { cn } from "@/lib/utils";
import { tasksConfig, CIRCUMFERENCE, MODES, SESSIONS_BEFORE_LONG, sessionHistory, Mode  } from "@/lib/dashboard-config";

const usePomodoro = () => {
  const [mode, setMode] = useState<Mode>("focus");
  const [seconds, setSeconds] = useState(MODES.focus.duration);
  const [running, setRunning] = useState(false);
  const [session, setSession] = useState(3);
  const [autoBreak, setAutoBreak] = useState(true);
  const [durations, setDurations] = useState({ focus: 25, short: 5, long: 15 });
  const [focusTask, setFocusTask] = useState(
    tasksConfig.find((t) => t.status === "inprogress") ?? tasksConfig[0],
  );
  const intervalRef = useRef<NodeJS.Timeout | null>(null);
  const totalSeconds = durations[mode] * 60;
  const progress = seconds / totalSeconds;
  const offset = CIRCUMFERENCE * (1 - progress);

  const formatTime = (s: number) => {
    const m = Math.floor(s / 60);
    const sec = s % 60;
    return `${String(m).padStart(2, "0")}:${String(sec).padStart(2, "0")}`;
  };

  const stop = useCallback(() => {
    if (intervalRef.current) clearInterval(intervalRef.current);
    setRunning(false);
  }, []);

  const handleEnd = useCallback(() => {
    stop();
    if (mode === "focus") {
      const next = session + 1;
      setSession(next);
      if (autoBreak) {
        const nextMode = next % SESSIONS_BEFORE_LONG === 0 ? "long" : "short";
        setMode(nextMode);
        setSeconds(durations[nextMode] * 60);
      }
    } else {
      setMode("focus");
      setSeconds(durations.focus * 60);
    }
  }, [mode, session, autoBreak, durations, stop]);

  useEffect(() => {
    if (running) {
      intervalRef.current = setInterval(() => {
        setSeconds((s) => {
          if (s <= 1) {
            handleEnd();
            return 0;
          }
          return s - 1;
        });
      }, 1000);
    }
    return () => {
      if (intervalRef.current) clearInterval(intervalRef.current);
    };
  }, [running, handleEnd]);

  const switchMode = (m: Mode) => {
    stop();
    setMode(m);
    setSeconds(durations[m] * 60);
  };

  const reset = () => {
    stop();
    setSeconds(durations[mode] * 60);
  };

  const skip = () => handleEnd();

  const adjustDuration = (key: keyof typeof durations, delta: number) => {
    setDurations((prev) => {
      const val = Math.max(1, Math.min(60, prev[key] + delta));
      const next = { ...prev, [key]: val };
      if (key === mode) {
        stop();
        setSeconds(val * 60);
      }
      return next;
    });
  };

  const modeColor = MODES[mode].color;

  return {
    mode,
    seconds,
    running,
    session,
    autoBreak,
    durations,
    focusTask,
    offset,
    formatTime,
    switchMode,
    reset,
    skip,
    adjustDuration,
    modeColor,
    setRunning,
    setAutoBreak,
  };
};

export default function PomodoroPage() {
  const {
    mode,
    seconds,
    running,
    session,
    autoBreak,
    durations,
    focusTask,
    offset,
    formatTime,
    switchMode,
    reset,
    skip,
    adjustDuration,
    modeColor,
    setRunning,
    setAutoBreak,
  } = usePomodoro();

  return (
    <div className="grid grid-cols-1 md:grid-cols-[1fr_300px] gap-3 items-start">
      {/* Left */}
      <div className="flex flex-col gap-3">
        {/* Timer card */}
        <div className="bg-muted/20 rounded-xl p-6">
          {/* Mode tabs */}
          <div className="flex bg-card border border-border rounded-md p-0.5 gap-0.5 mb-8">
            {(Object.keys(MODES) as Mode[]).map((m) => (
              <button
                key={m}
                onClick={() => switchMode(m)}
                className={cn(
                  "flex-1 py-1.5 rounded text-xs font-medium transition-all capitalize",
                  mode === m
                    ? "bg-muted text-foreground border border-border shadow-sm"
                    : "text-muted-foreground hover:text-foreground",
                )}>
                {m === "focus"
                  ? "Focus"
                  : m === "short"
                    ? "Short break"
                    : "Long break"}
              </button>
            ))}
          </div>

          {/* Circle timer */}
          <div className="flex flex-col items-center gap-6">
            <div className="relative w-48 h-48">
              <svg className="w-full h-full -rotate-90" viewBox="0 0 200 200">
                <circle
                  cx="100"
                  cy="100"
                  r="88"
                  fill="none"
                  stroke="hsl(var(--border))"
                  strokeWidth="6"
                />
                <circle
                  cx="100"
                  cy="100"
                  r="88"
                  fill="none"
                  stroke={modeColor}
                  strokeWidth="6"
                  strokeLinecap="round"
                  strokeDasharray={CIRCUMFERENCE}
                  strokeDashoffset={offset}
                  style={{ transition: "stroke-dashoffset 1s linear" }}
                />
              </svg>
              <div className="absolute inset-0 flex flex-col items-center justify-center gap-1">
                <span className="text-4xl font-medium text-foreground tracking-tight leading-none">
                  {formatTime(seconds)}
                </span>
                <span className="text-xs text-muted-foreground">
                  {MODES[mode].label}
                </span>
              </div>
            </div>

            {/* Session dots */}
            <div className="flex flex-col items-center gap-2">
              <div className="flex items-center gap-1.5">
                {Array.from({ length: SESSIONS_BEFORE_LONG }).map((_, i) => (
                  <div
                    key={i}
                    className="w-2 h-2 rounded-full transition-all"
                    style={{
                      background:
                        i < session % SESSIONS_BEFORE_LONG
                          ? modeColor
                          : "hsl(var(--border))",
                    }}
                  />
                ))}
              </div>
              <p className="text-xs text-muted-foreground">
                Session {session % SESSIONS_BEFORE_LONG || SESSIONS_BEFORE_LONG}{" "}
                of {SESSIONS_BEFORE_LONG}
                {" · "}
                {session % SESSIONS_BEFORE_LONG === SESSIONS_BEFORE_LONG - 1
                  ? "Long break next"
                  : "Short break next"}
              </p>
            </div>

            {/* Controls */}
            <div className="flex items-center gap-3">
              <button
                onClick={reset}
                className="w-10 h-10 rounded-full border border-border flex items-center justify-center text-muted-foreground hover:text-foreground transition-colors">
                <RotateCcw className="w-3.5 h-3.5" />
              </button>
              <button
                onClick={() => setRunning((r) => !r)}
                className="w-14 h-14 rounded-full flex items-center justify-center transition-opacity hover:opacity-90"
                style={{ background: "hsl(var(--foreground))" }}>
                {running ? (
                  <svg className="w-6 h-6" viewBox="0 0 24 24">
                    <rect x="6" y="4" width="4" height="16" fill={modeColor} />
                    <rect x="14" y="4" width="4" height="16" fill={modeColor} />
                  </svg>
                ) : (
                  <svg className="w-6 h-6" viewBox="0 0 24 24">
                    <polygon points="5 3 19 12 5 21 5 3" fill={modeColor} />
                  </svg>
                )}
              </button>
              <button
                onClick={skip}
                className="w-10 h-10 rounded-full border border-border flex items-center justify-center text-muted-foreground hover:text-foreground transition-colors">
                <RotateCw className="w-3.5 h-3.5" />
              </button>
            </div>
          </div>
        </div>

        {/* Focusing on */}
        <div className="bg-muted/20 rounded-xl p-5">
          <p className="text-xs font-medium uppercase tracking-widest text-muted-foreground mb-3">
            Focusing on
          </p>
          <div className="flex items-center gap-2.5 px-3 py-2.5 bg-card rounded-md border border-primary/20">
            <div className="w-3.5 h-3.5 rounded-full border border-border shrink-0" />
            <span className="text-sm text-foreground flex-1">
              {focusTask.title}
            </span>
            <span
              className={cn(
                "text-[10px] px-1.5 py-0.5 rounded-full border",
                focusTask.tag === "dev"
                  ? "text-blue-400 bg-blue-400/10 border-blue-400/20"
                  : "text-muted-foreground bg-muted/30 border-border",
              )}>
              {focusTask.tag}
            </span>
          </div>
          <button className="w-full mt-2 py-1.5 text-xs text-muted-foreground border border-dashed border-border rounded-md hover:text-foreground transition-colors">
            Change task
          </button>
        </div>
      </div>

      {/* Right */}
      <div className="flex flex-col gap-3">
        {/* Today stats */}
        <div className="bg-muted/20 rounded-xl p-5">
          <p className="text-xs font-medium uppercase tracking-widest text-muted-foreground mb-3">
            Today
          </p>
          <div className="grid grid-cols-2 gap-2">
            {[
              { value: String(session), label: "Sessions done" },
              { value: `${session * 25}m`, label: "Focus time" },
            ].map((s) => (
              <div
                key={s.label}
                className="bg-card border border-border rounded-md p-3">
                <p className="text-xl font-medium text-foreground leading-none mb-1">
                  {s.value}
                </p>
                <p className="text-xs text-muted-foreground">{s.label}</p>
              </div>
            ))}
          </div>
        </div>

        {/* Session history */}
        <div className="bg-muted/20 rounded-xl p-5">
          <p className="text-xs font-medium uppercase tracking-widest text-muted-foreground mb-3">
            Session history
          </p>
          <div className="flex flex-col gap-2">
            {sessionHistory.map((s) => (
              <div
                key={s.id}
                className="flex items-center gap-2.5 px-3 py-2.5 bg-card rounded-md border border-border">
                <div
                  className={cn(
                    "w-1.5 h-1.5 rounded-full shrink-0",
                    s.type === "focus" ? "bg-primary" : "bg-green-400",
                  )}
                />
                <div className="flex-1 min-w-0">
                  <p className="text-xs text-foreground truncate">{s.task}</p>
                  <p className="text-[10px] text-muted-foreground">
                    {s.duration} · {s.time}
                  </p>
                </div>
                <Check className="w-3 h-3 text-green-400 shrink-0" />
              </div>
            ))}
          </div>
        </div>

        {/* Settings */}
        <div className="bg-muted/20 rounded-xl p-5">
          <p className="text-xs font-medium uppercase tracking-widest text-muted-foreground mb-4">
            Timer settings
          </p>
          <div className="flex flex-col gap-3">
            {(["focus", "short", "long"] as const).map((key) => (
              <div key={key} className="flex items-center justify-between">
                <span className="text-xs text-muted-foreground capitalize">
                  {key === "focus"
                    ? "Focus duration"
                    : key === "short"
                      ? "Short break"
                      : "Long break"}
                </span>
                <div className="flex items-center gap-2">
                  <button
                    onClick={() => adjustDuration(key, -1)}
                    className="w-5 h-5 rounded border border-border flex items-center justify-center text-xs text-muted-foreground hover:text-foreground transition-colors">
                    −
                  </button>
                  <span className="text-xs font-medium text-foreground w-8 text-center">
                    {durations[key]}m
                  </span>
                  <button
                    onClick={() => adjustDuration(key, 1)}
                    className="w-5 h-5 rounded border border-border flex items-center justify-center text-xs text-muted-foreground hover:text-foreground transition-colors">
                    +
                  </button>
                </div>
              </div>
            ))}
            <div className="flex items-center justify-between pt-3 border-t border-border">
              <span className="text-xs text-muted-foreground">
                Auto-start breaks
              </span>
              <button
                onClick={() => setAutoBreak((a) => !a)}
                className={cn(
                  "w-8 h-4 rounded-full relative transition-colors",
                  autoBreak ? "bg-primary" : "bg-border",
                )}>
                <div
                  className={cn(
                    "absolute top-0.5 w-3 h-3 rounded-full bg-white transition-all",
                    autoBreak ? "right-0.5" : "left-0.5",
                  )}
                />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
