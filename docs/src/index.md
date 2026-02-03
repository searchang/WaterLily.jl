# WaterLily

## Introduction and Quickstart

```@docs
WaterLily
```

## Flow Playground

Use the mini interface below to drag WaterLily-inspired objects around a flowing field.
Collect all tokens into the glowing target zone to complete the level.
Swap between four challenge layouts to explore different object placements.

```@raw html
<section class="flow-playground">
  <div class="playground-header">
    <div>
      <h3>Flow Playground</h3>
      <p>Drag the bodies into the goal zone to clear the level.</p>
    </div>
    <div class="status">
      <span class="status-label">Level</span>
      <span id="flow-playground-level" class="status-value">1 / 4</span>
    </div>
  </div>
  <div class="level-controls" role="tablist" aria-label="Flow Playground levels">
    <button class="level-btn active" data-level="0" role="tab" aria-selected="true">Level 1</button>
    <button class="level-btn" data-level="1" role="tab" aria-selected="false">Level 2</button>
    <button class="level-btn" data-level="2" role="tab" aria-selected="false">Level 3</button>
    <button class="level-btn" data-level="3" role="tab" aria-selected="false">Level 4</button>
  </div>
  <div class="playground-board" id="flow-playground-board">
    <img src="assets/vort.png" alt="Flow field background" class="playground-bg" />
    <div class="goal-zone" id="flow-goal">
      <span>Goal</span>
    </div>
    <div class="tokens-layer" id="flow-tokens" aria-live="polite"></div>
  </div>
  <div class="playground-footer">
    <div class="status status-inline">
      <div>
        <span class="status-label">Tokens placed</span>
        <span id="flow-playground-score" class="status-value">0 / 3</span>
      </div>
      <div>
        <span class="status-label">Elapsed</span>
        <span id="flow-playground-time" class="status-value">0.0s</span>
      </div>
    </div>
    <div class="footer-actions">
      <button class="reset-btn" id="flow-reset">Reset</button>
      <button class="next-btn" id="flow-next">Next level</button>
    </div>
    <span class="hint" id="flow-hint">Tip: The flow field background is from a WaterLily example.</span>
  </div>
</section>
<style>
  .flow-playground {
    border: 1px solid #e3e6ef;
    border-radius: 16px;
    padding: 20px;
    margin: 24px 0 36px;
    background: #ffffff;
    box-shadow: 0 12px 30px rgba(17, 24, 39, 0.08);
    font-family: "Inter", "Helvetica Neue", Arial, sans-serif;
  }
  .flow-playground h3 {
    margin: 0 0 6px;
    font-size: 20px;
  }
  .flow-playground p {
    margin: 0;
    color: #516176;
  }
  .playground-header {
    display: flex;
    justify-content: space-between;
    gap: 16px;
    align-items: center;
    margin-bottom: 16px;
    flex-wrap: wrap;
  }
  .status {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 4px;
    background: #f6f7fb;
    padding: 10px 14px;
    border-radius: 10px;
    min-width: 120px;
  }
  .status-inline {
    flex-direction: row;
    align-items: center;
    gap: 24px;
    min-width: auto;
  }
  .status-inline .status-label,
  .status-inline .status-value {
    display: block;
    text-align: left;
  }
  .status-label {
    font-size: 12px;
    color: #70839a;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }
  .status-value {
    font-size: 18px;
    font-weight: 700;
    color: #2e3a51;
  }
  .level-controls {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 16px;
  }
  .level-btn {
    border: 1px solid #d6dbea;
    background: #ffffff;
    border-radius: 999px;
    padding: 8px 16px;
    font-weight: 600;
    color: #334155;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  .level-btn:hover {
    border-color: #7c8bf1;
    color: #1e3a8a;
  }
  .level-btn.active {
    background: #2563eb;
    color: #ffffff;
    border-color: #2563eb;
    box-shadow: 0 6px 16px rgba(37, 99, 235, 0.2);
  }
  .playground-board {
    position: relative;
    height: 360px;
    border-radius: 14px;
    overflow: hidden;
    border: 1px solid #e1e5f2;
    background: #0f172a;
  }
  .playground-bg {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0.6;
    pointer-events: none;
  }
  .goal-zone {
    position: absolute;
    left: 76%;
    top: 70%;
    width: 140px;
    height: 110px;
    border-radius: 18px;
    border: 2px dashed rgba(255, 255, 255, 0.7);
    display: flex;
    align-items: center;
    justify-content: center;
    color: #f8fafc;
    font-weight: 600;
    background: rgba(59, 130, 246, 0.22);
    box-shadow: 0 0 26px rgba(59, 130, 246, 0.45);
    backdrop-filter: blur(2px);
    transform: translate(-50%, -50%);
  }
  .tokens-layer {
    position: absolute;
    inset: 0;
  }
  .token {
    position: absolute;
    padding: 12px 18px;
    border-radius: 999px;
    color: #0f172a;
    font-weight: 600;
    cursor: grab;
    user-select: none;
    box-shadow: 0 10px 18px rgba(15, 23, 42, 0.2);
    border: 2px solid rgba(255, 255, 255, 0.85);
    touch-action: none;
  }
  .token:active {
    cursor: grabbing;
  }
  .token-sphere {
    background: linear-gradient(135deg, #fdfcfb, #e2d1ff);
  }
  .token-ring {
    background: linear-gradient(135deg, #fef3c7, #fdba74);
  }
  .token-wing {
    background: linear-gradient(135deg, #bbf7d0, #4ade80);
  }
  .token.locked {
    background: linear-gradient(135deg, #e2e8f0, #cbd5f5);
    color: #475569;
    cursor: default;
  }
  .playground-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 12px;
    margin-top: 16px;
    color: #52617a;
    font-size: 14px;
  }
  .reset-btn {
    border: none;
    background: #2563eb;
    color: white;
    padding: 10px 18px;
    border-radius: 10px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  .reset-btn:hover {
    transform: translateY(-1px);
    box-shadow: 0 6px 18px rgba(37, 99, 235, 0.25);
  }
  .reset-btn:active {
    transform: translateY(0);
  }
  .next-btn {
    border: none;
    background: #0f172a;
    color: #f8fafc;
    padding: 10px 18px;
    border-radius: 10px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  .next-btn:hover {
    transform: translateY(-1px);
    box-shadow: 0 6px 18px rgba(15, 23, 42, 0.25);
  }
  .next-btn:active {
    transform: translateY(0);
  }
  .footer-actions {
    display: flex;
    gap: 12px;
    align-items: center;
  }
  .hint {
    opacity: 0.75;
  }
</style>
<script>
  (function () {
    const board = document.getElementById("flow-playground-board");
    const goal = document.getElementById("flow-goal");
    const scoreLabel = document.getElementById("flow-playground-score");
    const timeLabel = document.getElementById("flow-playground-time");
    const levelLabel = document.getElementById("flow-playground-level");
    const hintLabel = document.getElementById("flow-hint");
    const resetButton = document.getElementById("flow-reset");
    const nextButton = document.getElementById("flow-next");
    const tokensLayer = document.getElementById("flow-tokens");
    const levelButtons = Array.from(document.querySelectorAll(".flow-playground .level-btn"));
    const levels = [
      {
        name: "Level 1",
        hint: "Place each object inside the glowing region.",
        goal: { left: 76, top: 70 },
        tokens: [
          { label: "Sphere", className: "token-sphere", left: 12, top: 60 },
          { label: "Ring", className: "token-ring", left: 30, top: 22 },
          { label: "Wing", className: "token-wing", left: 68, top: 26 },
        ],
      },
      {
        name: "Level 2",
        hint: "Guide the flow bodies around the vortex.",
        goal: { left: 18, top: 25 },
        tokens: [
          { label: "Cylinder", className: "token-sphere", left: 70, top: 70 },
          { label: "Fin", className: "token-wing", left: 48, top: 50 },
          { label: "Ring", className: "token-ring", left: 82, top: 18 },
        ],
      },
      {
        name: "Level 3",
        hint: "Stack the bodies in the downwash pocket.",
        goal: { left: 58, top: 22 },
        tokens: [
          { label: "Sphere", className: "token-sphere", left: 10, top: 30 },
          { label: "Foil", className: "token-wing", left: 20, top: 72 },
          { label: "Ring", className: "token-ring", left: 44, top: 56 },
        ],
      },
      {
        name: "Level 4",
        hint: "Final sprint: clear the lane and finish the run.",
        goal: { left: 84, top: 48 },
        tokens: [
          { label: "Cone", className: "token-sphere", left: 18, top: 58 },
          { label: "Ring", className: "token-ring", left: 34, top: 20 },
          { label: "Wing", className: "token-wing", left: 62, top: 68 },
        ],
      },
    ];
    let tokens = [];
    let initialPositions = [];
    let activeLevelIndex = 0;
    let startTime = performance.now();
    let timerId = null;
    let levelComplete = false;

    function updateScore() {
      const placed = tokens.filter((token) => token.classList.contains("locked")).length;
      scoreLabel.textContent = `${placed} / ${tokens.length}`;
      if (placed === tokens.length) {
        levelComplete = true;
        cancelAnimationFrame(timerId);
      }
    }

    function isOverlappingGoal(token) {
      const tokenRect = token.getBoundingClientRect();
      const goalRect = goal.getBoundingClientRect();
      const centerX = tokenRect.left + tokenRect.width / 2;
      const centerY = tokenRect.top + tokenRect.height / 2;
      return (
        centerX > goalRect.left &&
        centerX < goalRect.right &&
        centerY > goalRect.top &&
        centerY < goalRect.bottom
      );
    }

    function lockToken(token) {
      token.classList.add("locked");
      const goalRect = goal.getBoundingClientRect();
      const boardRect = board.getBoundingClientRect();
      const left = goalRect.left - boardRect.left + 16 + Math.random() * (goalRect.width - 40);
      const top = goalRect.top - boardRect.top + 16 + Math.random() * (goalRect.height - 40);
      token.style.left = `${left}px`;
      token.style.top = `${top}px`;
      token.setAttribute("aria-grabbed", "true");
      updateScore();
    }

    function unlockTokens() {
      tokens.forEach((token, index) => {
        token.classList.remove("locked");
        token.style.left = initialPositions[index].left;
        token.style.top = initialPositions[index].top;
        token.removeAttribute("aria-grabbed");
      });
      levelComplete = false;
      startTime = performance.now();
      timeLabel.textContent = "0.0s";
      tickTimer();
      updateScore();
    }

    function tickTimer() {
      if (levelComplete) return;
      const elapsed = (performance.now() - startTime) / 1000;
      timeLabel.textContent = `${elapsed.toFixed(1)}s`;
      timerId = requestAnimationFrame(tickTimer);
    }

    function attachDragHandlers(token) {
      let offsetX = 0;
      let offsetY = 0;
      let isDragging = false;
      let activePointerId = null;

      const onPointerMove = (event) => {
        if (!isDragging || token.classList.contains("locked")) return;
        const boardRect = board.getBoundingClientRect();
        const newLeft = event.clientX - boardRect.left - offsetX;
        const newTop = event.clientY - boardRect.top - offsetY;
        token.style.left = `${Math.max(0, Math.min(newLeft, boardRect.width - token.offsetWidth))}px`;
        token.style.top = `${Math.max(0, Math.min(newTop, boardRect.height - token.offsetHeight))}px`;
      };

      const onPointerUp = () => {
        if (!isDragging) return;
        isDragging = false;
        if (activePointerId !== null) {
          token.releasePointerCapture(activePointerId);
        }
        activePointerId = null;
        if (isOverlappingGoal(token)) {
          lockToken(token);
        }
      };

      token.addEventListener("pointerdown", (event) => {
        if (token.classList.contains("locked")) return;
        activePointerId = event.pointerId;
        token.setPointerCapture(activePointerId);
        isDragging = true;
        const rect = token.getBoundingClientRect();
        offsetX = event.clientX - rect.left;
        offsetY = event.clientY - rect.top;
      });

      token.addEventListener("pointermove", onPointerMove);
      token.addEventListener("pointerup", onPointerUp);
      token.addEventListener("pointercancel", onPointerUp);
    }

    function renderLevel(index) {
      const level = levels[index];
      activeLevelIndex = index;
      cancelAnimationFrame(timerId);
      tokensLayer.innerHTML = "";
      tokens = level.tokens.map((tokenData, tokenIndex) => {
        const token = document.createElement("div");
        token.className = `draggable token ${tokenData.className}`;
        token.dataset.token = `${tokenIndex + 1}`;
        token.textContent = tokenData.label;
        token.style.left = `${tokenData.left}%`;
        token.style.top = `${tokenData.top}%`;
        token.setAttribute("role", "button");
        token.setAttribute("aria-label", `${tokenData.label} token`);
        tokensLayer.appendChild(token);
        attachDragHandlers(token);
        return token;
      });
      initialPositions = level.tokens.map((tokenData) => ({
        left: `${tokenData.left}%`,
        top: `${tokenData.top}%`,
      }));
      goal.style.left = `${level.goal.left}%`;
      goal.style.top = `${level.goal.top}%`;
      levelLabel.textContent = `${index + 1} / ${levels.length}`;
      hintLabel.textContent = `Tip: ${level.hint}`;
      timeLabel.textContent = "0.0s";
      levelButtons.forEach((button) => {
        const isActive = Number(button.dataset.level) === index;
        button.classList.toggle("active", isActive);
        button.setAttribute("aria-selected", isActive.toString());
      });
      levelComplete = false;
      startTime = performance.now();
      tickTimer();
      updateScore();
    }

    levelButtons.forEach((button) => {
      button.addEventListener("click", () => {
        renderLevel(Number(button.dataset.level));
      });
    });

    nextButton.addEventListener("click", () => {
      const nextIndex = (activeLevelIndex + 1) % levels.length;
      renderLevel(nextIndex);
    });

    renderLevel(activeLevelIndex);
    resetButton.addEventListener("click", unlockTokens);
  })();
</script>
```

## Types Methods and Functions
```@meta
CurrentModule = WaterLily
```

```@index
```

```@autodocs
Modules = [WaterLily]
Order   = [:constant, :type, :function, :macro]
```
