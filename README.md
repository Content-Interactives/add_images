# Add Images

React (JSX) applet: **apple** and **orange** icons (Tailwind-drawn circles) render in counts **`apples`** and **`oranges`**. The learner sets three steppers **`inputApples`**, **`inputOranges`**, **`inputAnswer`** (each **1–10**) to match **`a + b = a+b`**, then **Check!**. Correct → **canvas-confetti**, success line + random praise, new problem after **3s**. Wrong → brief **shake** on the button (inline `shake` keyframes from `AddImages.css`).

**Live site:** [https://content-interactives.github.io/add_images](https://content-interactives.github.io/add_images)

Standards and curriculum: [Standards.md](Standards.md).

---

## Stack

| Layer | Notes |
|--------|--------|
| Build | Vite 7, `@vitejs/plugin-react` |
| UI | React 19 (`.jsx` only—**not** TypeScript) |
| Styling | Tailwind 3, `AddImages.css` (`@keyframes shake`) |
| Effects | `canvas-confetti` |
| Deploy | `gh-pages -d dist`, `predeploy` → `vite build` |

---

## Layout

```
vite.config.js          # base: '/add_images/'
src/
  main.jsx → App.jsx → components/AddImages.jsx
  components/AddImages.css
  components/ui/reused-ui/Container.jsx
```

---

## `generateImages`

1. **`total`** = uniform **3…10** (`Math.floor(Math.random() * 8) + 3`).
2. **`maxApples`** = `min(total - 1, 9)`; **`minApples`** = `max(1, total - 9)` so **`oranges = total - apples`** stays in **[1, 9]** when **`apples`** is chosen in range.
3. **`randomApples`** uniform in **[minApples, maxApples]**; **`oranges = total - randomApples`**.
4. **`setApples` / `setOranges`** drive the two `Array.from({ length: n })` grids (no shared `Items` module—markup is inlined).

---

## `checkAnswer`

- **Correct iff** **`inputApples + inputOranges === inputAnswer`** (numeric equality, not string compare to a canonical equation).
- For a correct response, the learner must match both **counts** to the picture and the **sum** to **`apples + oranges`** (initial steppers default to **1**, so the first check is usually wrong until adjusted).

**Timeouts:** **3s** then reset inputs to **1** and **`generateImages()`**; no guard against overlapping timeouts if the user spams Check (low risk in practice).

---

## `Container`

- **`showSoundButton={true}`** with **no `onSound`**.
- **`showResetButton={false}`**.

---

## `vite.config.js`

**`base: '/add_images/'`** must match the GitHub Pages project path.

---

## Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Vite dev server |
| `npm run build` | `dist/` |
| `npm run preview` | Preview build |
| `npm run lint` | ESLint |
| `npm run deploy` | Build + gh-pages |

---

## Embedding

- Fixed-height **`Container`** chrome (~500px); fruit grid is a centered flex wrap of **48×48**-ish icons (`w-12 h-12`).

---

## Maintenance

- Remove unused **`useRef`** and **`useCallback`** imports from `AddImages.jsx`.
- **`animate-pulse`** on the Check button while shaking may fight the inline **`shake`** animation—pick one if motion looks odd.
- Optionally block **Check** while **`showAnswer`** is true (already only success UI is shown).
