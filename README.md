import React, { useEffect, useMemo, useState } from "react";
import { motion, AnimatePresence } from "framer-motion";
import {
  ArrowLeft,
  Crown,
  Info,
  Medal,
  Moon,
  MousePointer2,
  RotateCcw,
  Search,
  Settings2,
  Shield,
  Sun,
  Trophy,
  Users,
  X,
} from "lucide-react";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";

const seasonOptions = ["S1", "S2", "S3", "S4", "ALL-TIME"];

const themes = {
  blue: { name: "Blue", primary: "#3b82f6", soft: "rgba(59,130,246,.18)", border: "rgba(59,130,246,.32)" },
  red: { name: "Red", primary: "#ef4444", soft: "rgba(239,68,68,.18)", border: "rgba(239,68,68,.32)" },
  gold: { name: "Gold", primary: "#d4af37", soft: "rgba(212,175,55,.18)", border: "rgba(212,175,55,.32)" },
  orange: { name: "Orange", primary: "#f59e0b", soft: "rgba(245,158,11,.18)", border: "rgba(245,158,11,.32)" },
  yellow: { name: "Yellow", primary: "#eab308", soft: "rgba(234,179,8,.18)", border: "rgba(234,179,8,.32)" },
  mint: { name: "Mint", primary: "#6ee7b7", soft: "rgba(110,231,183,.18)", border: "rgba(110,231,183,.32)" },
  green: { name: "Green", primary: "#22c55e", soft: "rgba(34,197,94,.18)", border: "rgba(34,197,94,.32)" },
  pink: { name: "Pink", primary: "#ec4899", soft: "rgba(236,72,153,.18)", border: "rgba(236,72,153,.32)" },
  purple: { name: "Purple", primary: "#8b5cf6", soft: "rgba(139,92,246,.18)", border: "rgba(139,92,246,.32)" },
  tan: { name: "Taupe", primary: "#c4ab87", soft: "rgba(196,171,135,.18)", border: "rgba(196,171,135,.32)" },
  silver: { name: "Silver", primary: "#cbd5e1", soft: "rgba(203,213,225,.18)", border: "rgba(203,213,225,.32)" },
};

const players = [
  { username: "Mil", team: "Cold Hearts", pts: 1184, ast: 246, reb: 143, stl: 61, gp: 24, ppg: 49.3, apg: 10.3, rpg: 6.0, spg: 2.5, eff: 330.2 },
  { username: "Jayfr", team: "Bape City Reapers", pts: 946, ast: 201, reb: 96, stl: 58, gp: 22, ppg: 43.0, apg: 9.1, rpg: 4.4, spg: 2.6, eff: 301.8 },
  { username: "Aimz", team: "Bape YNG Dreamers", pts: 1022, ast: 114, reb: 156, stl: 49, gp: 21, ppg: 48.7, apg: 5.4, rpg: 7.4, spg: 2.3, eff: 297.5 },
  { username: "Solo", team: "RWE", pts: 887, ast: 311, reb: 109, stl: 67, gp: 23, ppg: 38.6, apg: 13.5, rpg: 4.7, spg: 2.9, eff: 342.4 },
  { username: "Kxri", team: "Fear of God", pts: 812, ast: 133, reb: 190, stl: 42, gp: 20, ppg: 40.6, apg: 6.7, rpg: 9.5, spg: 2.1, eff: 288.0 },
  { username: "Nova", team: "Cold Hearts", pts: 760, ast: 145, reb: 101, stl: 52, gp: 19, ppg: 40.0, apg: 7.6, rpg: 5.3, spg: 2.7, eff: 284.3 },
];

const hallOfFame = [
  {
    name: "Legacy Vault",
    era: "Hall of Fame opens soon",
    image: "https://images.unsplash.com/photo-1513106580091-1d82408b8cd6?auto=format&fit=crop&w=800&q=80",
    stats: { slots: "0", added: "0", season: "S4", status: "soon" },
    accolades: ["New class pending", "More legends later"],
  },
];

const seasons = [
  { season: "S3", champion: "RWE", mvp: "Alex", finalists: ["Spongebob Jelly Fam", "RWE"] },
  { season: "S2", champion: "RWE", mvp: "Solo", finalists: ["Cold Hearts", "RWE"] },
  { season: "S1", champion: "Fear of God", mvp: "Fear", finalists: ["Fear of God", "Bape City Reapers"] },
];

const teams = [
  "Rolling Loud",
  "Renn Elite",
  "Fear of God",
  "RWE",
  "Bape City Reapers",
  "Cold Hearts",
  "Bape YNG Dreamers",
  "Spongebob Jelly Fam",
  "Dimond Doves",
  "Bape Blue Checks",
  "TY Elite EYBL",
  "FaZe",
];

const standings = [
  { rank: 1, team: "Rolling Loud", tag: "#1 Seed" },
  { rank: 2, team: "Renn Elite", tag: "#2 Seed" },
  { rank: 3, team: "Fear of God", tag: "#3 Seed" },
  { rank: 4, team: "TY Elite EYBL", tag: "#4 Seed" },
  { rank: 5, team: "Bape City Reapers", tag: "#5 Seed" },
  { rank: 6, team: "RWE", tag: "#6 Seed" },
  { rank: 7, team: "Bape YNG Dreamers", tag: "#7 Seed" },
  { rank: 8, team: "Spongebob Jelly Fam", tag: "#8 Seed" },
  { rank: 9, team: "Dimond Doves", tag: "#9 Seed" },
  { rank: 10, team: "Bape Blue Checks", tag: "#10 Seed" },
  { rank: 11, team: "Cold Hearts", tag: "#11 Seed" },
  { rank: 12, team: "FaZe", tag: "#12 Seed" },
];

const discordRules = [
  "Respect everyone in chat, VC, and game lobbies.",
  "No slurs, harassment, threats, or personal attacks.",
  "No spam, mass pinging, or flooding channels.",
  "Keep league arguments out of general chat when staff says move on.",
  "No fake scores, fake screenshots, or misleading stat posts.",
  "Do not impersonate staff, players, or teams.",
  "Use the correct channels for trades, media, standings, and tickets.",
  "No NSFW, graphic, or shock content.",
  "No advertising outside approved promo channels.",
  "Staff decisions can be appealed respectfully through tickets.",
];

const navItems = [
  { key: "home", label: "home" },
  { key: "stats", label: "stats" },
  { key: "compare", label: "compare" },
  { key: "legacy", label: "hof" },
  { key: "history", label: "history" },
  { key: "info", label: "info" },
];

const statColumns: [keyof (typeof players)[number], string][] = [
  ["pts", "PTS"],
  ["ast", "AST"],
  ["reb", "REB"],
  ["stl", "STL"],
  ["gp", "GP"],
  ["ppg", "PPG"],
  ["apg", "APG"],
  ["rpg", "RPG"],
  ["spg", "SPG"],
  ["eff", "EFF"],
];

type Theme = (typeof themes)[keyof typeof themes];

type Styles = ReturnType<typeof useThemeStyles>;

function useThemeStyles(theme: Theme, lightMode: boolean) {
  return {
    bg: lightMode ? "#f8fafc" : "#020617",
    panel: lightMode ? "rgba(255,255,255,.88)" : "rgba(255,255,255,.05)",
    panelSoft: lightMode ? "rgba(255,255,255,.96)" : "rgba(255,255,255,.04)",
    border: lightMode ? "rgba(15,23,42,.08)" : "rgba(255,255,255,.10)",
    text: lightMode ? "#0f172a" : "#ffffff",
    subtext: lightMode ? "#475569" : "#a1a1aa",
    muted: lightMode ? "#64748b" : "#71717a",
    tile: lightMode ? "rgba(248,250,252,.98)" : "rgba(0,0,0,.30)",
    searchBg: lightMode ? "#ffffff" : "rgba(255,255,255,.06)",
  };
}

function PillToggle({ options, value, onChange, styles, theme }: { options: string[]; value: string; onChange: (v: string) => void; styles: Styles; theme: Theme }) {
  return (
    <div className="flex items-center gap-2 rounded-full border p-1" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}>
      {options.map((option) => {
        const active = value === option;
        return (
          <button
            key={option}
            onClick={() => onChange(option)}
            className="rounded-full px-4 py-2 text-xs font-semibold tracking-wide transition"
            style={{
              backgroundColor: active ? theme.primary : "transparent",
              color: active ? "white" : styles.subtext,
              boxShadow: active ? `0 0 30px ${theme.soft}` : "none",
            }}
          >
            {option}
          </button>
        );
      })}
    </div>
  );
}

function StatChip({ label, value, active = false, styles, theme }: { label: string; value: string | number; active?: boolean; styles: Styles; theme: Theme }) {
  return (
    <div
      className="inline-flex min-w-[60px] items-center justify-center rounded-full px-3 py-1 text-sm font-bold"
      style={{
        backgroundColor: active ? theme.soft : styles.panelSoft,
        color: active ? theme.primary : styles.text,
        boxShadow: active ? `0 0 18px ${theme.soft}` : "none",
      }}
    >
      <span>{value}</span>
      <span className="sr-only">{label}</span>
    </div>
  );
}

function LeagueLogo({ theme }: { theme: Theme }) {
  return (
    <div
      className="relative mx-auto flex h-24 w-24 items-center justify-center rounded-[28px] border"
      style={{
        borderColor: theme.border,
        backgroundColor: theme.soft,
        boxShadow: `0 0 35px ${theme.soft}, inset 0 0 20px rgba(255,255,255,.04)`,
      }}
    >
      <svg viewBox="0 0 64 64" className="h-14 w-14" aria-hidden="true">
        <rect x="13" y="8" width="38" height="48" rx="12" fill="none" stroke={theme.primary} strokeWidth="4" />
        <rect x="21" y="16" width="22" height="32" rx="10" fill="none" stroke={theme.primary} strokeWidth="3.5" />
        <path d="M32 16v32" stroke={theme.primary} strokeWidth="3" strokeLinecap="round" />
        <path d="M21 32h22" stroke={theme.primary} strokeWidth="3" strokeLinecap="round" />
        <path d="M24 22c5 4 11 4 16 0" fill="none" stroke={theme.primary} strokeWidth="3" strokeLinecap="round" />
        <path d="M24 42c5-4 11-4 16 0" fill="none" stroke={theme.primary} strokeWidth="3" strokeLinecap="round" />
      </svg>
      <div className="absolute inset-0 rounded-[28px]" style={{ boxShadow: `0 0 50px ${theme.soft}` }} />
    </div>
  );
}

function LinkButton({ href, label, theme, icon }: { href: string; label: string; theme: Theme; icon?: string }) {
  return (
    <button
      type="button"
      onClick={() => window.open(href, "_blank", "noopener,noreferrer")}
      className="inline-flex items-center gap-3 rounded-full px-7 py-4 text-base font-bold text-white shadow-lg transition hover:scale-[1.02]"
      style={{ backgroundColor: theme.primary }}
    >
      {icon ? <span className="text-lg">{icon}</span> : null}
      {label}
    </button>
  );
}

function LoadingHome({ setCurrent, styles, theme }: { setCurrent: (v: string) => void; styles: Styles; theme: Theme }) {
  return (
    <div className="mx-auto flex min-h-[72vh] max-w-7xl items-center justify-center px-6 sm:px-10 lg:px-16">
      <div className="w-full max-w-4xl rounded-[2.5rem] border p-8 text-center sm:p-12" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
        <motion.div initial={{ scale: 0.95, opacity: 0 }} animate={{ scale: 1, opacity: 1 }} transition={{ duration: 0.45 }}>
          <LeagueLogo theme={theme} />
          <div className="mt-8 text-5xl font-black tracking-tight sm:text-7xl" style={{ color: styles.text }}>OTE</div>
          <p className="mx-auto mt-4 max-w-2xl text-lg" style={{ color: styles.subtext }}>
            A custom Roblox OTE basketball league hub with standings, player stats, hall of fame tracking, season history, and Discord rules.
          </p>
          <div className="mt-6 flex flex-wrap justify-center gap-4">
            <LinkButton href="https://discord.gg/VFngjYmg" label="Join Us" theme={theme} />
            <LinkButton href="https://www.tiktok.com/@otebasketballroleplay" label="Media" theme={theme} />
          </div>
          <div className="mx-auto mt-8 h-3 w-full max-w-xl overflow-hidden rounded-full" style={{ backgroundColor: styles.panelSoft }}>
            <motion.div className="h-full rounded-full" style={{ backgroundColor: theme.primary }} initial={{ width: "0%" }} animate={{ width: "100%" }} transition={{ duration: 1.8, ease: "easeOut" }} />
          </div>
          <div className="mt-8 flex justify-center gap-4">
            <Button className="rounded-full px-8 py-6 text-base font-bold text-white" style={{ backgroundColor: theme.primary }} onClick={() => setCurrent("info")}>open info</Button>
            <Button variant="outline" className="rounded-full px-8 py-6 text-base" style={{ borderColor: styles.border, color: styles.text, backgroundColor: styles.panelSoft }} onClick={() => setCurrent("stats")}>player stats</Button>
          </div>
        </motion.div>
      </div>
    </div>
  );
}

function NavBar({ current, setCurrent, theme, styles, lightMode, setLightMode, setSettingsOpen }: { current: string; setCurrent: (v: string) => void; theme: Theme; styles: Styles; lightMode: boolean; setLightMode: React.Dispatch<React.SetStateAction<boolean>>; setSettingsOpen: React.Dispatch<React.SetStateAction<boolean>> }) {
  return (
    <div className="sticky top-4 z-40 mx-auto w-[96%] max-w-7xl rounded-full border px-4 py-4 backdrop-blur-xl" style={{ borderColor: theme.border, backgroundColor: lightMode ? "rgba(255,255,255,.88)" : "rgba(2,6,23,.75)" }}>
      <div className="flex items-center justify-between gap-3">
        <button onClick={() => setCurrent("home")} className="text-3xl font-black tracking-tight" style={{ color: theme.primary }}>ote</button>
        <div className="hidden items-center gap-8 md:flex">
          {navItems.map((item) => (
            <button key={item.key} onClick={() => setCurrent(item.key)} className="relative text-base font-semibold transition" style={{ color: current === item.key ? styles.text : styles.subtext }}>
              {item.label}
              {current === item.key ? <motion.div layoutId="nav-line" className="absolute -bottom-2 left-0 right-0 h-0.5 rounded-full" style={{ backgroundColor: theme.primary }} /> : null}
            </button>
          ))}
        </div>
        <div className="flex items-center gap-2">
          <button className="flex h-11 w-11 items-center justify-center rounded-full border transition" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft, color: styles.text }} onClick={() => setLightMode((v) => !v)}>
            {lightMode ? <Moon className="h-4 w-4" /> : <Sun className="h-4 w-4" />}
          </button>
          <button className="flex h-11 w-11 items-center justify-center rounded-full text-white shadow-lg" style={{ backgroundColor: theme.primary }}>
            <Search className="h-5 w-5" />
          </button>
          <button className="flex h-11 w-11 items-center justify-center rounded-full border transition" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft, color: styles.text }} onClick={() => setSettingsOpen(true)}>
            <Settings2 className="h-4 w-4" />
          </button>
        </div>
      </div>
    </div>
  );
}

function SettingsPanel({ open, onClose, themeKey, setThemeKey, stickyHeader, setStickyHeader, lightMode, setLightMode, quickSearch, setQuickSearch, reducedMotion, setReducedMotion, highContrast, setHighContrast, styles, theme }: { open: boolean; onClose: () => void; themeKey: keyof typeof themes; setThemeKey: React.Dispatch<React.SetStateAction<keyof typeof themes>>; stickyHeader: boolean; setStickyHeader: React.Dispatch<React.SetStateAction<boolean>>; lightMode: boolean; setLightMode: React.Dispatch<React.SetStateAction<boolean>>; quickSearch: boolean; setQuickSearch: React.Dispatch<React.SetStateAction<boolean>>; reducedMotion: boolean; setReducedMotion: React.Dispatch<React.SetStateAction<boolean>>; highContrast: boolean; setHighContrast: React.Dispatch<React.SetStateAction<boolean>>; styles: Styles; theme: Theme }) {
  if (!open) return null;
  return (
    <div className="fixed inset-0 z-50 flex justify-end bg-black/40 backdrop-blur-sm">
      <div className="h-full w-full max-w-md overflow-y-auto border-l p-6" style={{ borderColor: styles.border, backgroundColor: styles.bg }}>
        <div className="mb-8 flex items-center justify-between">
          <div className="text-3xl font-black italic" style={{ color: styles.text }}>SETTINGS</div>
          <button onClick={onClose} className="rounded-full p-2" style={{ color: styles.subtext }}><X className="h-5 w-5" /></button>
        </div>
        <div className="space-y-8">
          <section>
            <div className="mb-4 text-xs font-bold uppercase tracking-[0.35em]" style={{ color: styles.muted }}>appearance</div>
            <div className="rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
              <div className="text-lg font-bold" style={{ color: styles.text }}>Site Accent Color</div>
              <div className="mt-1 text-sm" style={{ color: styles.subtext }}>Choose your accent color.</div>
              <div className="mt-5 grid grid-cols-4 gap-4">
                {Object.entries(themes).map(([key, value]) => {
                  const active = key === themeKey;
                  return (
                    <button key={key} onClick={() => setThemeKey(key as keyof typeof themes)} className="relative h-14 w-14 rounded-full border-2 transition" style={{ backgroundColor: value.primary, borderColor: active ? styles.text : "transparent" }}>
                      {active ? <div className="absolute inset-0 m-auto flex h-full w-full items-center justify-center text-lg font-black text-white">✓</div> : null}
                    </button>
                  );
                })}
              </div>
            </div>
          </section>
          <section className="space-y-4">
            {[
              { label: "Light Mode", value: lightMode, set: setLightMode },
              { label: "Sticky Header", value: stickyHeader, set: setStickyHeader },
              { label: "Reduced Motion", value: reducedMotion, set: setReducedMotion },
              { label: "High Contrast", value: highContrast, set: setHighContrast },
              { label: "Quick Search", value: quickSearch, set: setQuickSearch },
            ].map((item) => (
              <div key={item.label} className="flex items-center justify-between rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
                <div>
                  <div className="text-lg font-bold" style={{ color: styles.text }}>{item.label}</div>
                  <div className="text-sm" style={{ color: styles.subtext }}>Toggle this setting on or off.</div>
                </div>
                <button onClick={() => item.set(!item.value)} className="relative h-8 w-14 rounded-full transition" style={{ backgroundColor: item.value ? theme.primary : styles.panelSoft }}>
                  <span className="absolute top-1 h-6 w-6 rounded-full bg-white transition" style={{ left: item.value ? 30 : 4 }} />
                </button>
              </div>
            ))}
          </section>
        </div>
      </div>
    </div>
  );
}

function InfoPage({ setCurrent, styles, theme }: { setCurrent: (v: string) => void; styles: Styles; theme: Theme }) {
  return (
    <div className="mx-auto max-w-7xl px-6 pb-20 pt-10 sm:px-10 lg:px-16">
      <div className="grid gap-6 lg:grid-cols-[1.05fr_.95fr]">
        <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
          <CardContent className="p-8">
            <div className="mb-4 inline-flex items-center gap-2 rounded-full px-4 py-2 text-sm font-bold" style={{ backgroundColor: theme.soft, color: theme.primary }}>
              <Info className="h-4 w-4" /> league info
            </div>
            <h1 className="text-5xl font-black tracking-tight sm:text-6xl" style={{ color: styles.text }}>About the league</h1>
            <p className="mt-5 max-w-2xl text-lg" style={{ color: styles.subtext }}>
              This is a custom Roblox OTE basketball league community hub. It tracks standings, player stats, hall of fame spots, season history, rules, and team info in one place.
            </p>
            <p className="mt-4 max-w-2xl text-base" style={{ color: styles.subtext }}>
              Use this site to check rankings, compare players, look through past seasons, and join the league Discord.
            </p>
            <div className="mt-8 flex flex-wrap gap-4">
              <Button className="rounded-full px-8 py-6 text-base font-bold text-white" style={{ backgroundColor: theme.primary }} onClick={() => setCurrent("stats")}>view stats</Button>
              <Button variant="outline" className="rounded-full px-8 py-6 text-base" style={{ borderColor: styles.border, color: styles.text, backgroundColor: styles.panelSoft }} onClick={() => setCurrent("history")}>season history</Button>
            </div>
          </CardContent>
        </Card>
        <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
          <CardContent className="p-8">
            <div className="text-sm font-bold uppercase tracking-[0.25em]" style={{ color: styles.muted }}>discord</div>
            <div className="mt-3 text-3xl font-black" style={{ color: styles.text }}>Join the server</div>
            <div className="mt-5 flex flex-wrap gap-4">
              <LinkButton href="https://discord.gg/VFngjYmg" label="Join Us" theme={theme} />
              <LinkButton href="https://www.tiktok.com/@otebasketballroleplay" label="Media" theme={theme} />
            </div>
            <div className="mt-6 grid gap-4 sm:grid-cols-2">
              <div className="rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}><div className="text-sm font-bold uppercase tracking-[0.2em]" style={{ color: styles.muted }}>season</div><div className="mt-2 text-3xl font-black" style={{ color: styles.text }}>S4</div></div>
              <div className="rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}><div className="text-sm font-bold uppercase tracking-[0.2em]" style={{ color: styles.muted }}>current #1</div><div className="mt-2 text-3xl font-black" style={{ color: styles.text }}>Rolling Loud</div></div>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}

function StatsPage({ season, setSeason, setCurrent, styles, theme }: { season: string; setSeason: (v: string) => void; setCurrent: (v: string) => void; styles: Styles; theme: Theme }) {
  return (
    <div className="mx-auto max-w-7xl px-6 pb-20 pt-10 sm:px-10 lg:px-16">
      <button onClick={() => setCurrent("info")} className="mb-8 inline-flex items-center gap-2 text-sm font-bold tracking-wide" style={{ color: theme.primary }}>
        <ArrowLeft className="h-4 w-4" /> INFO
      </button>
      <div className="mb-8 flex flex-col gap-6 lg:flex-row lg:items-center lg:justify-between">
        <div>
          <h2 className="text-5xl font-black tracking-tight sm:text-6xl" style={{ color: theme.primary }}>player statistics</h2>
          <p className="mt-3" style={{ color: styles.subtext }}>Sortable leaderboard layout for your league site.</p>
        </div>
        <div className="flex flex-wrap items-center gap-3">
          <PillToggle options={seasonOptions} value={season} onChange={setSeason} styles={styles} theme={theme} />
          <div className="flex items-center gap-2 rounded-full border px-4 py-2" style={{ borderColor: styles.border, backgroundColor: styles.searchBg }}><Search className="h-4 w-4" style={{ color: styles.subtext }} /><span className="text-sm" style={{ color: styles.subtext }}>Search</span></div>
        </div>
      </div>
      <Card className="overflow-hidden rounded-[2rem] border shadow-2xl" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
        <CardContent className="p-0">
          <div className="max-h-[680px] overflow-auto">
            <table className="w-full min-w-[980px] text-left">
              <thead style={{ backgroundColor: styles.panelSoft }}>
                <tr className="text-xs uppercase tracking-[0.22em]" style={{ color: styles.muted }}>
                  <th className="px-5 py-5">Player</th>
                  {statColumns.map(([key, label]) => <th key={String(key)} className="px-5 py-5">{label}</th>)}
                </tr>
              </thead>
              <tbody>
                {players.map((player, index) => (
                  <tr key={player.username} className="border-t text-sm transition" style={{ borderColor: styles.border, color: styles.text }}>
                    <td className="px-5 py-5"><div><div className="font-bold">{player.username}</div><div className="mt-1 text-xs" style={{ color: styles.subtext }}>{player.team}</div></div></td>
                    {statColumns.map(([key]) => {
                      const isFeatured = (index === 0 && (key === "pts" || key === "eff")) || (player.username === "Solo" && (key === "ast" || key === "apg")) || (player.username === "Aimz" && key === "ppg") || (player.username === "Mil" && key === "spg");
                      return <td key={String(key)} className="px-5 py-5 font-semibold"><StatChip label={String(key)} value={player[key]} active={isFeatured} styles={styles} theme={theme} /></td>;
                    })}
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}

function ComparePage({ season, setSeason, setCurrent, styles, theme }: { season: string; setSeason: (v: string) => void; setCurrent: (v: string) => void; styles: Styles; theme: Theme }) {
  const [leftSearch, setLeftSearch] = useState("Mil");
  const [rightSearch, setRightSearch] = useState("Solo");
  const left = useMemo(() => players.find((p) => p.username.toLowerCase() === leftSearch.toLowerCase()) || players[0], [leftSearch]);
  const right = useMemo(() => players.find((p) => p.username.toLowerCase() === rightSearch.toLowerCase()) || players[3], [rightSearch]);
  const categories = [["PPG", left.ppg, right.ppg], ["APG", left.apg, right.apg], ["RPG", left.rpg, right.rpg], ["SPG", left.spg, right.spg], ["EFF", left.eff, right.eff]];

  return (
    <div className="mx-auto max-w-7xl px-6 pb-20 pt-10 sm:px-10 lg:px-16">
      <button onClick={() => setCurrent("info")} className="mb-8 inline-flex items-center gap-2 text-sm font-bold tracking-wide" style={{ color: theme.primary }}>
        <ArrowLeft className="h-4 w-4" /> INFO
      </button>
      <div className="mb-8 flex flex-col gap-6 xl:flex-row xl:items-center xl:justify-between">
        <h2 className="text-5xl font-black tracking-tight sm:text-6xl" style={{ color: theme.primary }}>compare players</h2>
        <div className="flex flex-wrap items-center gap-3">
          <div className="flex items-center rounded-full border p-1" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}>
            <button className="rounded-full p-3" style={{ backgroundColor: theme.soft, color: theme.primary }}><Users className="h-4 w-4" /></button>
            <button className="rounded-full p-3" style={{ color: styles.subtext }}><RotateCcw className="h-4 w-4" /></button>
          </div>
          <PillToggle options={seasonOptions} value={season} onChange={setSeason} styles={styles} theme={theme} />
          <div className="flex items-center gap-2 rounded-full border px-4 py-2" style={{ borderColor: styles.border, backgroundColor: styles.searchBg }}><Search className="h-4 w-4" style={{ color: styles.subtext }} /><span className="text-sm" style={{ color: styles.subtext }}>Search</span></div>
        </div>
      </div>
      <div className="rounded-[2rem] border p-6 md:p-8" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}>
        <div className="grid gap-6 lg:grid-cols-2">
          {[left, right].map((player, idx) => (
            <Card key={`${player.username}-${idx}`} className="overflow-hidden rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
              <CardContent className="p-6">
                <div className="mb-5 flex items-center justify-between gap-4">
                  <div><div className="text-2xl font-black" style={{ color: styles.text }}>{player.username}</div><div className="text-sm" style={{ color: styles.subtext }}>{player.team}</div></div>
                  <Badge className="rounded-full" style={{ backgroundColor: theme.soft, color: theme.primary }}>{idx === 0 ? "Player A" : "Player B"}</Badge>
                </div>
                <Input value={idx === 0 ? leftSearch : rightSearch} onChange={(e) => (idx === 0 ? setLeftSearch(e.target.value) : setRightSearch(e.target.value))} placeholder="Search player username" className="mb-5 rounded-2xl" style={{ borderColor: styles.border, backgroundColor: styles.searchBg, color: styles.text }} />
                <div className="grid grid-cols-2 gap-4 sm:grid-cols-5">
                  {[[player.ppg, "PPG"], [player.apg, "APG"], [player.rpg, "RPG"], [player.spg, "SPG"], [player.eff, "EFF"]].map(([value, label]) => (
                    <div key={label} className="rounded-2xl p-4" style={{ backgroundColor: styles.tile, minWidth: 0 }}>
                      <div className="text-2xl font-black leading-none sm:text-3xl" style={{ color: styles.text }}>{Number(value).toFixed(1).replace(/\.0$/, "")}</div>
                      <div className="mt-2 text-xs tracking-[0.2em]" style={{ color: styles.muted }}>{label}</div>
                    </div>
                  ))}
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
        <div className="mt-6 rounded-[2rem] border p-6" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
          <div className="mb-4 text-center text-2xl font-black" style={{ color: styles.text }}>head to head</div>
          <div className="space-y-4">
            {categories.map(([label, a, b]) => {
              const total = Number(a) + Number(b);
              const leftWidth = total === 0 ? 50 : (Number(a) / total) * 100;
              return (
                <div key={label}>
                  <div className="mb-2 flex items-center justify-between text-sm font-semibold" style={{ color: styles.subtext }}><span>{label}</span><span>{a} • {b}</span></div>
                  <div className="flex h-4 overflow-hidden rounded-full" style={{ backgroundColor: styles.panelSoft }}>
                    <div style={{ width: `${leftWidth}%`, backgroundColor: theme.primary }} />
                    <div style={{ width: `${100 - leftWidth}%`, backgroundColor: styles.border }} />
                  </div>
                </div>
              );
            })}
          </div>
        </div>
      </div>
    </div>
  );
}

function LegacyPage({ setCurrent, styles, theme }: { setCurrent: (v: string) => void; styles: Styles; theme: Theme }) {
  const [showRecords, setShowRecords] = useState(false);

  const recordSections = [
    {
      title: "Game Records",
      rows: [
        { label: "Most points in a game", value: "---", stat: "PTS", holder: "Add player" },
        { label: "Most assists in a game", value: "---", stat: "AST", holder: "Add player" },
        { label: "Most rebounds in a game", value: "---", stat: "REB", holder: "Add player" },
        { label: "Most steals in a game", value: "---", stat: "STL", holder: "Add player" },
      ],
    },
    {
      title: "Season Records",
      rows: [
        { label: "Highest PPG in a season", value: "---", stat: "PPG", holder: "Add player" },
        { label: "Highest APG in a season", value: "---", stat: "APG", holder: "Add player" },
        { label: "Highest RPG in a season", value: "---", stat: "RPG", holder: "Add player" },
        { label: "Highest SPG in a season", value: "---", stat: "SPG", holder: "Add player" },
      ],
    },
    {
      title: "Career Records",
      rows: [
        { label: "Most career points", value: "---", stat: "PTS", holder: "Add player" },
        { label: "Most career assists", value: "---", stat: "AST", holder: "Add player" },
        { label: "Most career rebounds", value: "---", stat: "REB", holder: "Add player" },
        { label: "Most career steals", value: "---", stat: "STL", holder: "Add player" },
      ],
    },
    {
      title: "Awards / Titles",
      rows: [
        { label: "Most rings", value: "---", stat: "RINGS", holder: "Add player" },
        { label: "Most MVP awards", value: "---", stat: "MVP", holder: "Add player" },
        { label: "Most finals MVP awards", value: "---", stat: "FMVP", holder: "Add player" },
        { label: "Most defensive awards", value: "---", stat: "DPOTY", holder: "Add player" },
      ],
    },
  ];

  return (
    <div className="mx-auto max-w-7xl px-6 pb-20 pt-10 sm:px-10 lg:px-16">
      <button onClick={() => setCurrent("info")} className="mb-8 inline-flex items-center gap-2 text-sm font-bold tracking-wide" style={{ color: theme.primary }}>
        <ArrowLeft className="h-4 w-4" /> INFO
      </button>

      <div className="mb-8 flex flex-col gap-4 lg:flex-row lg:items-center lg:justify-between">
        <h2 className="text-5xl font-black tracking-tight sm:text-6xl" style={{ color: theme.primary }}>hof</h2>
        <div className="flex items-center gap-3 rounded-full border p-1" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}>
          <button onClick={() => setShowRecords(false)} className="rounded-full px-10 py-3 text-sm font-bold transition" style={{ backgroundColor: showRecords ? "transparent" : theme.primary, color: showRecords ? styles.subtext : "white" }}>HOF</button>
          <button onClick={() => setShowRecords(true)} className="rounded-full px-10 py-3 text-sm font-bold transition" style={{ backgroundColor: showRecords ? theme.primary : "transparent", color: showRecords ? "white" : styles.subtext }}>RECORDS</button>
        </div>
      </div>

      {!showRecords ? (
        <div className="grid gap-6 lg:grid-cols-1">
          {hallOfFame.map((member, index) => (
            <motion.div key={member.name} whileHover={{ y: -4 }}>
              <Card className="overflow-hidden rounded-[2rem] border" style={{ borderColor: index === 0 ? "rgba(234,179,8,.45)" : styles.border, backgroundColor: styles.panel }}>
                <div className="relative h-[420px] bg-cover bg-center" style={{ backgroundImage: `linear-gradient(to top, rgba(0,0,0,.82), rgba(0,0,0,.24)), url(${member.image})` }}>
                  <div className="absolute bottom-5 left-5 right-5">
                    <div className="text-5xl font-black text-yellow-400">{member.name}</div>
                    <div className="mt-2 text-sm uppercase tracking-[0.28em] text-zinc-300">accolades</div>
                    <div className="mt-3 flex flex-wrap gap-2">
                      {member.accolades.map((acc) => <Badge key={acc} className="rounded-md border border-yellow-500/20 bg-yellow-500/10 text-yellow-300">{acc}</Badge>)}
                    </div>
                  </div>
                </div>
                <CardContent className="border-t p-5" style={{ borderColor: styles.border }}>
                  <div className="grid grid-cols-2 gap-4 rounded-[1.5rem] p-5" style={{ backgroundColor: styles.tile }}>
                    {Object.entries(member.stats).map(([k, v]) => (
                      <div key={k}><div className="text-4xl font-black" style={{ color: styles.text }}>{v}</div><div className="mt-1 text-xs uppercase tracking-[0.25em] text-yellow-400">{k}</div></div>
                    ))}
                  </div>
                  <div className="mt-4 text-center text-xs uppercase tracking-[0.35em]" style={{ color: styles.muted }}>{member.era}</div>
                </CardContent>
              </Card>
            </motion.div>
          ))}
        </div>
      ) : (
        <div className="space-y-8">
          {recordSections.map((section) => (
            <Card key={section.title} className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
              <CardContent className="p-6 md:p-8">
                <div className="mb-6 text-2xl font-black" style={{ color: styles.text }}>{section.title}</div>
                <div className="overflow-auto">
                  <table className="w-full min-w-[700px] text-left">
                    <thead>
                      <tr className="border-b text-xs uppercase tracking-[0.22em]" style={{ borderColor: styles.border, color: styles.muted }}>
                        <th className="pb-4">Record</th>
                        <th className="pb-4">Value</th>
                        <th className="pb-4">Stat</th>
                        <th className="pb-4">Holder</th>
                      </tr>
                    </thead>
                    <tbody>
                      {section.rows.map((row) => (
                        <tr key={row.label} className="border-b last:border-b-0" style={{ borderColor: styles.border }}>
                          <td className="py-5 font-semibold" style={{ color: styles.text }}>{row.label}</td>
                          <td className="py-5 text-2xl font-black" style={{ color: theme.primary }}>{row.value}</td>
                          <td className="py-5 text-sm font-bold uppercase tracking-[0.18em]" style={{ color: styles.muted }}>{row.stat}</td>
                          <td className="py-5" style={{ color: styles.subtext }}>{row.holder}</td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
      )}
    </div>
  );
}

function HistoryPage({ setCurrent, styles, theme }: { setCurrent: (v: string) => void; styles: Styles; theme: Theme }) {
  const historyCards = [
    { title: "achievements", sub: "legacy vault", action: "view section", key: "legacy" },
    { title: "players", sub: "stats leaderboard", action: "view section", key: "stats" },
    { title: "rules", sub: "discord rules", action: "view section", key: "info" },
    { title: "history", sub: "season archive", action: "active", key: "history" },
  ];
  return (
    <div className="mx-auto max-w-7xl px-6 pb-20 pt-10 sm:px-10 lg:px-16">
      <button onClick={() => setCurrent("info")} className="mb-8 inline-flex items-center gap-2 text-sm font-bold tracking-wide" style={{ color: theme.primary }}>
        <ArrowLeft className="h-4 w-4" /> INFO
      </button>
      <div className="mb-10"><h2 className="text-5xl font-black tracking-tight sm:text-6xl" style={{ color: theme.primary }}>discover more</h2></div>
      <div className="grid gap-6 md:grid-cols-2">
        {historyCards.map((card) => (
          <button key={card.title} onClick={() => { if (card.key === "stats") setCurrent("stats"); if (card.key === "legacy") setCurrent("legacy"); if (card.key === "info") setCurrent("info"); }} className="group rounded-[2rem] border p-8 text-left transition" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
            <div className="flex min-h-[150px] flex-col justify-between">
              <div><div className="text-3xl font-black sm:text-4xl" style={{ color: styles.text }}>{card.title}</div><div className="mt-4 text-sm font-bold uppercase tracking-[0.2em]" style={{ color: styles.muted }}>{card.action}</div></div>
              <div className="flex items-center justify-between"><div className="text-sm" style={{ color: styles.subtext }}>{card.sub}</div><div className="flex h-12 w-12 items-center justify-center rounded-full text-white transition group-hover:scale-105" style={{ backgroundColor: theme.primary }}>→</div></div>
            </div>
          </button>
        ))}
      </div>
      <div className="mt-12 space-y-8">
        {seasons.map((item) => (
          <Card key={item.season} className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
            <CardContent className="p-6 md:p-8">
              <div className="mb-6 flex flex-col gap-4 lg:flex-row lg:items-center lg:justify-between">
                <div className="text-5xl font-black" style={{ color: styles.text }}>{item.season}</div>
                <div className="rounded-full px-5 py-3 text-sm font-bold" style={{ backgroundColor: theme.soft, color: theme.primary }}>season archive</div>
              </div>
              <div className="grid gap-4 md:grid-cols-3">
                <div className="rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.tile }}><div className="text-xs uppercase tracking-[0.25em]" style={{ color: styles.muted }}>Champion</div><div className="mt-2 text-2xl font-black" style={{ color: styles.text }}>{item.champion}</div></div>
                <div className="rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.tile }}><div className="text-xs uppercase tracking-[0.25em]" style={{ color: styles.muted }}>MVP</div><div className="mt-2 text-2xl font-black" style={{ color: styles.text }}>{item.mvp}</div></div>
                <div className="rounded-[1.5rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.tile }}><div className="text-xs uppercase tracking-[0.25em]" style={{ color: styles.muted }}>Finalists</div><div className="mt-2 text-2xl font-black" style={{ color: styles.text }}>{item.finalists.join(" vs ")}</div></div>
              </div>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}

function RulesSection({ styles, theme }: { styles: Styles; theme: Theme }) {
  return (
    <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
      <CardContent className="p-8">
        <div className="mb-4 inline-flex items-center gap-2 rounded-full px-4 py-2 text-sm font-bold" style={{ backgroundColor: theme.soft, color: theme.primary }}><Shield className="h-4 w-4" /> discord rules</div>
        <div className="grid gap-4 md:grid-cols-2">
          {discordRules.map((rule, index) => (
            <div key={rule} className="rounded-[1.4rem] border p-5" style={{ borderColor: styles.border, backgroundColor: styles.panelSoft }}>
              <div className="text-sm font-bold" style={{ color: theme.primary }}>Rule {index + 1}</div>
              <div className="mt-2 text-base font-semibold" style={{ color: styles.text }}>{rule}</div>
            </div>
          ))}
        </div>
      </CardContent>
    </Card>
  );
}

function TeamSection({ styles, theme }: { styles: Styles; theme: Theme }) {
  return (
    <div className="mx-auto mt-8 max-w-7xl px-6 sm:px-10 lg:px-16">
      <div className="mb-5 flex items-center justify-between gap-4">
        <div>
          <div className="text-sm font-bold uppercase tracking-[0.25em]" style={{ color: styles.muted }}>Franchises</div>
          <div className="mt-2 text-4xl font-black" style={{ color: styles.text }}>league teams</div>
          <div className="mt-2 text-sm" style={{ color: styles.subtext }}>Standings order shown below</div>
        </div>
        <div className="rounded-full px-4 py-2 text-sm font-bold" style={{ backgroundColor: theme.soft, color: theme.primary }}>12 teams</div>
      </div>
      <div className="grid gap-4 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
        {standings.map((item) => (
          <Card key={item.team} className="rounded-[1.7rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}>
            <CardContent className="p-5">
              <div className="mb-3 flex items-center justify-between"><div className="text-xs font-bold uppercase tracking-[0.18em]" style={{ color: styles.muted }}>Rank</div><div className="text-sm font-bold" style={{ color: theme.primary }}>#{item.rank}</div></div>
              <div className="text-2xl font-black" style={{ color: styles.text }}>{item.team}</div>
              <div className="mt-2 text-sm" style={{ color: styles.subtext }}>{item.tag}</div>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}

function SiteFooterStats({ styles, theme }: { styles: Styles; theme: Theme }) {
  return (
    <div className="mx-auto mt-8 grid max-w-7xl gap-5 px-6 sm:grid-cols-2 sm:px-10 lg:grid-cols-4 lg:px-16">
      <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}><CardContent className="p-5"><div className="mb-2 flex items-center gap-3" style={{ color: theme.primary }}><Trophy className="h-5 w-5" /><span className="text-sm font-bold uppercase tracking-[0.22em]">Champions</span></div><div className="text-4xl font-black" style={{ color: styles.text }}>4</div><div className="mt-1 text-sm" style={{ color: styles.subtext }}>1 live + 3 completed</div></CardContent></Card>
      <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}><CardContent className="p-5"><div className="mb-2 flex items-center gap-3" style={{ color: theme.primary }}><Users className="h-5 w-5" /><span className="text-sm font-bold uppercase tracking-[0.22em]">Players</span></div><div className="text-4xl font-black" style={{ color: styles.text }}>60+</div><div className="mt-1 text-sm" style={{ color: styles.subtext }}>Custom OTE database</div></CardContent></Card>
      <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}><CardContent className="p-5"><div className="mb-2 flex items-center gap-3" style={{ color: theme.primary }}><Medal className="h-5 w-5" /><span className="text-sm font-bold uppercase tracking-[0.22em]">Awards</span></div><div className="text-4xl font-black" style={{ color: styles.text }}>15+</div><div className="mt-1 text-sm" style={{ color: styles.subtext }}>Achievements and trophies</div></CardContent></Card>
      <Card className="rounded-[2rem] border" style={{ borderColor: styles.border, backgroundColor: styles.panel }}><CardContent className="p-5"><div className="mb-2 flex items-center gap-3" style={{ color: theme.primary }}><Crown className="h-5 w-5" /><span className="text-sm font-bold uppercase tracking-[0.22em]">Teams</span></div><div className="text-4xl font-black" style={{ color: styles.text }}>12</div><div className="mt-1 text-sm" style={{ color: styles.subtext }}>OTE franchises added</div></CardContent></Card>
    </div>
  );
}

function CustomCursor({ x, y, styles }: { x: number; y: number; styles: Styles }) {
  return (
    <div className="pointer-events-none fixed z-[100] hidden md:block" style={{ left: x, top: y, transform: "translate(-6px, -6px) rotate(-12deg)" }}>
      <MousePointer2 className="h-8 w-8 fill-current drop-shadow-xl" style={{ color: styles.text }} />
    </div>
  );
}

export default function NBLLLeagueSite() {
  const [current, setCurrent] = useState("home");
  const [season, setSeason] = useState("S4");
  const [themeKey, setThemeKey] = useState<keyof typeof themes>("blue");
  const [settingsOpen, setSettingsOpen] = useState(false);
  const [lightMode, setLightMode] = useState(false);
  const [stickyHeader, setStickyHeader] = useState(true);
  const [quickSearch, setQuickSearch] = useState(true);
  const [reducedMotion, setReducedMotion] = useState(false);
  const [highContrast, setHighContrast] = useState(false);
  const [cursor, setCursor] = useState({ x: 0, y: 0 });

  const theme = themes[themeKey];
  const styles = useThemeStyles(theme, lightMode);

  useEffect(() => {
    document.documentElement.style.scrollBehavior = reducedMotion ? "auto" : "smooth";
  }, [reducedMotion]);

  useEffect(() => {
    const move = (e: MouseEvent) => setCursor({ x: e.clientX, y: e.clientY });
    window.addEventListener("mousemove", move);

    const styleEl = document.createElement("style");
    styleEl.setAttribute("data-custom-cursor", "true");
    styleEl.innerHTML = `html, body, button, a, input, textarea, select, * { cursor: none !important; }`;
    document.head.appendChild(styleEl);

    return () => {
      window.removeEventListener("mousemove", move);
      styleEl.remove();
    };
  }, []);

  return (
    <div className="min-h-screen" style={{ backgroundColor: styles.bg, color: styles.text, cursor: "none" }}>
      <div className="fixed inset-0" style={{ background: lightMode ? `radial-gradient(circle at top, ${theme.soft}, transparent 36%), linear-gradient(to bottom, #f8fafc, #e2e8f0)` : `radial-gradient(circle at top, ${theme.soft}, transparent 38%), linear-gradient(to bottom, rgba(5,10,25,0.95), #000)` }} />
      <CustomCursor x={cursor.x} y={cursor.y} styles={styles} />
      <div className="relative z-10 pb-16">
        <div className={stickyHeader ? "sticky top-4 z-40 pt-4" : "pt-4"}>
          <NavBar current={current} setCurrent={setCurrent} theme={theme} styles={styles} lightMode={lightMode} setLightMode={setLightMode} setSettingsOpen={setSettingsOpen} />
        </div>
        {current !== "home" ? (
          <div className="mx-auto mt-6 flex max-w-7xl flex-col justify-between gap-4 px-6 sm:px-10 lg:flex-row lg:px-16">
            <PillToggle options={seasonOptions} value={season} onChange={setSeason} styles={styles} theme={theme} />
            <div className="flex items-center gap-2 rounded-full border px-4 py-3" style={{ borderColor: styles.border, backgroundColor: styles.searchBg }}>
              <Search className="h-4 w-4" style={{ color: styles.subtext }} />
              <span className="text-sm" style={{ color: styles.subtext }}>{quickSearch ? "Quick Search enabled" : "Search"}</span>
            </div>
          </div>
        ) : null}
        <AnimatePresence mode="wait">
          <motion.div key={current} initial={{ opacity: 0, y: 18 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0, y: -10 }} transition={{ duration: reducedMotion ? 0 : 0.25 }}>
            {current === "home" ? <LoadingHome setCurrent={setCurrent} styles={styles} theme={theme} /> : null}
            {current === "info" ? (
              <>
                <InfoPage setCurrent={setCurrent} styles={styles} theme={theme} />
                <div className="mx-auto mt-8 max-w-7xl px-6 sm:px-10 lg:px-16"><RulesSection styles={styles} theme={theme} /></div>
                <TeamSection styles={styles} theme={theme} />
                <SiteFooterStats styles={styles} theme={theme} />
              </>
            ) : null}
            {current === "stats" ? <StatsPage season={season} setSeason={setSeason} setCurrent={setCurrent} styles={styles} theme={theme} /> : null}
            {current === "compare" ? <ComparePage season={season} setSeason={setSeason} setCurrent={setCurrent} styles={styles} theme={theme} /> : null}
            {current === "legacy" ? <LegacyPage setCurrent={setCurrent} styles={styles} theme={theme} /> : null}
            {current === "history" ? <HistoryPage setCurrent={setCurrent} styles={styles} theme={theme} /> : null}
          </motion.div>
        </AnimatePresence>
      </div>
      <SettingsPanel
        open={settingsOpen}
        onClose={() => setSettingsOpen(false)}
        themeKey={themeKey}
        setThemeKey={setThemeKey}
        stickyHeader={stickyHeader}
        setStickyHeader={setStickyHeader}
        lightMode={lightMode}
        setLightMode={setLightMode}
        quickSearch={quickSearch}
        setQuickSearch={setQuickSearch}
        reducedMotion={reducedMotion}
        setReducedMotion={setReducedMotion}
        highContrast={highContrast}
        setHighContrast={setHighContrast}
        styles={styles}
        theme={theme}
      />
    </div>
  );
}

const __tests = (() => {
  const uniqueTeams = new Set(teams);
  if (uniqueTeams.size !== teams.length) throw new Error("Teams must be unique");
  if (standings[0].team !== "Rolling Loud") throw new Error("Rolling Loud should be #1");
  if (standings[3].team !== "TY Elite EYBL") throw new Error("TY Elite EYBL should be #4");
  if (seasons[0].finalists.join(" vs ") !== "Spongebob Jelly Fam vs RWE") throw new Error("S3 finalists mismatch");
  if (seasons[2].champion !== "Fear of God") throw new Error("S1 champion mismatch");
  return true;
})();
