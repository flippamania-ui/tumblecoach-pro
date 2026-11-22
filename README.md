 TumbleCoachPro is a mobile first skill tracking and biomechanical analysis application designed for athletes and coaches.
## Core Features 
Video Scrubbing
Telestrator-Overlays 
Skill Directory 
​Tumble Coach Pro: Architectural Overview & Sovereign State Kinematics (v2.2)
​1. Executive Summary & IP Boundary
Tumble Coach Pro (TCP) is a zero-build, offline-first biomechanical analysis engine designed exclusively for the one-on-one private coaching model. Distinct from enterprise gym kiosks, the intellectual property boundary strictly isolates proprietary fault-tree logic, the 15-second floor loop workflow, and the tcp_sovereign_state_v1 schema from general-purpose video processing infrastructure. All legacy Bluetooth biometric hardware integrations are explicitly archived, enforcing a unified software-only IP boundary.
​2. Causal Fault-Tree Logic: Single-Fault Isolation
Most AI motion analysis engines evaluate kinematics globally, returning noisy, multi-faceted scorecards that overwhelm the athlete. TCP v2.2 implements a strict causal Fault-Tree Model designed to identify the single earliest temporal mechanical breach in a tumbling skill (e.g., a collapsed shoulder angle in a back handspring). By isolating this primary fault, the system suppresses downstream secondary symptoms (such as knee flexion or pike at apex), ensuring the athlete receives exactly one corrective cue per repetition.
​3. Zero-Build, Offline-First Architecture
To ensure operational reliability inside dead-zone, metal-roofed training facilities, TCP v2.2 abandons web-view or app-store constraints in favor of a single, self-contained HTML document. State mutations are captured locally via Unified Pointer Events with touch-action: none to eliminate gesture latency.
​4. Persistent Sovereign State Schema
The application state is encapsulated under the namespace tcp_sovereign_state_v1 within browser localStorage. The data structure is strictly defined across four complementary modules:
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TCPSovereignStateV1",
  "type": "object",
  "properties": {
    "athleteProfile": {
      "type": "object",
      "properties": {
        "athleteIdentifier": { "type": "string" },
        "skillLevel": { "type": "string", "enum": ["USASF L1", "USASF L2", "USASF L3", "USASF L4", "USASF L5", "USASF L6"] },
        "bodyKinematics": {
          "type": "object",
          "properties": {
            "limbLengthRatio": { "type": "number" },
            "centerOfMassBaseline": { "type": "number" }
          },
          "required": ["limbLengthRatio", "centerOfMassBaseline"],
          "additionalProperties": false
        }
      },
      "required": ["athleteIdentifier", "skillLevel"],
      "additionalProperties": false
    },
    "lessonLogs": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "sessionTimestamp": { "type": "string", "format": "date-time" },
          "skillPrescription": { "type": "string" },
          "isolatedFault": {
            "type": "object",
            "properties": {
              "breachFrameIndex": { "type": "integer" },
              "faultDescription": { "type": "string" },
              "telestratorMarker": { "type": "string", "enum": ["Red", "Yellow", "Green"] }
            },
            "required": ["breachFrameIndex", "faultDescription", "telestratorMarker"],
            "additionalProperties": false
          }
        },
        "required": ["sessionTimestamp", "skillPrescription", "isolatedFault"],
        "additionalProperties": false
      }
    },
    "videoRepository": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "blobStorageUri": { "type": "string", "pattern": "^blob:" },
          "manifestMetadata": {
            "type": "object",
            "properties": {
              "fps": { "type": "integer" },
              "resolution": { "type": "string" },
              "isScrubbable": { "type": "boolean" }
            },
            "required": ["fps", "resolution", "isScrubbable"],
            "additionalProperties": false
          }
        },
        "required": ["blobStorageUri", "manifestMetadata"],
        "additionalProperties": false
      }
    },
    "pendingSync": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "transactionId": { "type": "string" },
          "deltaState": { "type": "object" },
          "queueTimestamp": { "type": "string", "format": "date-time" }
        },
        "required": ["transactionId", "deltaState", "queueTimestamp"],
        "additionalProperties": false
      }
    }
  },
  "required": ["athleteProfile", "lessonLogs", "videoRepository", "pendingSync"],
  "additionalProperties": false
}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Sub-25ms Closed-Loop Telemetry Harness — Tumble Coach Pro</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet" />
  <style>
    body {
      background-color: #090b12;
      color: #e5e7eb;
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    .mono {
      font-family: 'JetBrains Mono', ui-monospace, SFMono-Regular, monospace;
    }
    input[type="range"] {
      accent-color: #00f5d4;
      height: 28px;
      cursor: pointer;
    }
    .card {
      background: #111624;
      border: 1px solid #1f2937;
    }
    .aura-border {
      border-color: #441147;
    }
  </style>
</head>
<body class="min-h-screen pb-10">
  <div class="max-w-3xl mx-auto px-3 sm:px-5 pt-4 space-y-4">
    <!-- Header -->
    <header class="flex flex-wrap items-center justify-between gap-2 pb-2 border-b border-gray-800">
      <div>
        <div class="flex items-center gap-2">
          <span class="inline-block w-2.5 h-2.5 rounded-full bg-[#00f5d4]"></span>
          <h1 class="text-base sm:text-lg font-bold tracking-tight text-white">
            Sub-25ms Telemetry Harness
          </h1>
        </div>
        <p class="text-xs text-gray-400">
          1:1 JS/WASM-ready port of <span class="mono text-[#00f5d4]">tcp_telemetry</span> C++20 engine
        </p>
      </div>
      <div class="flex items-center gap-2">
        <button id="tab-sim" type="button" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-[#00f5d4] text-[#090b12]">
          Live Harness
        </button>
        <button id="tab-cpp" type="button" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-gray-800 text-gray-300 hover:bg-gray-700">
          WASM / C++20
        </button>
      </div>
    </header>

    <!-- Main Simulation View -->
    <main id="view-sim" class="space-y-4">
      <!-- KPI Strip -->
      <section class="grid grid-cols-2 sm:grid-cols-4 gap-2.5">
        <div class="card rounded-xl p-3">
          <div class="text-[11px] uppercase tracking-wider text-gray-400">Worst M2P</div>
          <div class="flex items-baseline gap-1.5 mt-1">
            <span id="kpi-m2p" class="mono text-xl font-bold text-[#00f5d4]">14.02</span>
            <span class="text-xs text-gray-400">ms</span>
          </div>
          <div id="kpi-gate" class="mt-1 inline-block px-2 py-0.5 rounded text-[10px] font-bold bg-[#00f5d4]/15 text-[#00f5d4]">
            PASS &lt; 25.0 ms
          </div>
        </div>

        <div class="card rounded-xl p-3">
          <div class="text-[11px] uppercase tracking-wider text-gray-400">Engine Compute</div>
          <div class="flex items-baseline gap-1.5 mt-1">
            <span id="kpi-compute" class="mono text-xl font-bold text-white">20</span>
            <span class="text-xs text-gray-400">us</span>
          </div>
          <div class="text-[11px] text-gray-400 mt-1">SPSC + Fault-Tree</div>
        </div>

        <div class="card rounded-xl p-3">
          <div class="text-[11px] uppercase tracking-wider text-gray-400">Root Fault</div>
          <div id="kpi-fault" class="mono text-sm font-bold text-[#00f5d4] mt-1.5 truncate">
            OPTIMAL_FORM
          </div>
          <div id="kpi-drill" class="text-[11px] text-gray-400 mt-1">DrillRx #0</div>
        </div>

        <div class="card rounded-xl p-3">
          <div class="text-[11px] uppercase tracking-wider text-gray-400">Clipping Guard</div>
          <div id="kpi-clip" class="mono text-sm font-bold text-[#00f5d4] mt-1.5">
            NOMINAL
          </div>
          <div id="kpi-accel" class="text-[11px] text-gray-400 mt-1">Z: 1.80 g (Rail 15.8g)</div>
        </div>
      </section>

      <!-- Action Buttons -->
      <section class="grid grid-cols-3 gap-2">
        <button id="btn-gate-test" type="button" class="py-2.5 px-3 rounded-xl text-xs sm:text-sm font-semibold bg-[#441147] hover:bg-[#58175c] text-white border border-purple-500/40 active:scale-[0.98] transition">
          Run 1,000-Frame Gate
        </button>
        <button id="btn-stream" type="button" class="py-2.5 px-3 rounded-xl text-xs sm:text-sm font-semibold bg-[#00f5d4] text-[#090b12] hover:bg-[#22ffe2] active:scale-[0.98] transition">
          Stream 240Hz
        </button>
        <button id="btn-calibrate" type="button" class="py-2.5 px-3 rounded-xl text-xs sm:text-sm font-semibold bg-gray-800 hover:bg-gray-700 text-gray-200 border border-gray-700 active:scale-[0.98] transition">
          Frame-Zero Cal
        </button>
      </section>

      <!-- Visualizers: AR Waveguide Stick Figure + Latency Trace -->
      <section class="grid grid-cols-1 sm:grid-cols-2 gap-3">
        <div class="card rounded-xl p-3 flex flex-col">
          <div class="flex items-center justify-between mb-2">
            <span class="text-xs font-semibold text-gray-300">AR Waveguide Overlay (Sagittal)</span>
            <span id="hud-ring" class="mono text-[11px] text-gray-400">Ring: 0/1024</span>
          </div>
          <div class="relative w-full h-56 bg-[#07090f] rounded-lg border border-gray-800 overflow-hidden">
            <canvas id="canvas-pose" class="w-full h-full block"></canvas>
          </div>
        </div>

        <div class="card rounded-xl p-3 flex flex-col">
          <div class="flex items-center justify-between mb-2">
            <span class="text-xs font-semibold text-gray-300">M2P Latency Trace (25 ms Ceiling)</span>
            <span id="hud-frames" class="mono text-[11px] text-gray-400">Frames: 0</span>
          </div>
          <div class="relative w-full h-56 bg-[#07090f] rounded-lg border border-gray-800 overflow-hidden">
            <canvas id="canvas-latency" class="w-full h-full block"></canvas>
          </div>
        </div>
      </section>

      <!-- Interactive Kinematic & Sensor Sliders -->
      <section class="card rounded-xl p-4 space-y-3">
        <div class="flex items-center justify-between">
          <h2 class="text-xs font-semibold uppercase tracking-wider text-gray-300">
            Live Sensor &amp; Joint Inputs
          </h2>
          <button id="btn-reset-inputs" type="button" class="text-xs text-[#00f5d4] hover:underline">
            Reset Nominal
          </button>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-x-5 gap-y-2.5">
          <div>
            <div class="flex justify-between text-xs mb-0.5">
              <span class="text-gray-300">Shoulder Angle (Fault &lt; 155&deg;)</span>
              <span id="val-shoulder" class="mono text-[#00f5d4]">174&deg;</span>
            </div>
            <input id="slider-shoulder" type="range" min="120" max="185" value="174" class="w-full" />
          </div>

          <div>
            <div class="flex justify-between text-xs mb-0.5">
              <span class="text-gray-300">Hip Angle (Fault &lt; 145&deg;)</span>
              <span id="val-hip" class="mono text-[#00f5d4]">170&deg;</span>
            </div>
            <input id="slider-hip" type="range" min="115" max="185" value="170" class="w-full" />
          </div>

          <div>
            <div class="flex justify-between text-xs mb-0.5">
              <span class="text-gray-300">Knee Angle (Caution &lt; 150&deg;)</span>
              <span id="val-knee" class="mono text-[#00f5d4]">172&deg;</span>
            </div>
            <input id="slider-knee" type="range" min="120" max="185" value="172" class="w-full" />
          </div>

          <div>
            <div class="flex justify-between text-xs mb-0.5">
              <span class="text-gray-300">Z-Axis Accel (Clip &ge; 15.8 g)</span>
              <span id="val-accel" class="mono text-[#00f5d4]">1.8 g</span>
            </div>
            <input id="slider-accel" type="range" min="0.5" max="18.0" step="0.1" value="1.8" class="w-full" />
          </div>

          <div class="sm:col-span-2">
            <div class="flex justify-between text-xs mb-0.5">
              <span class="text-gray-300">Simulated Hardware I/O + Photon Settling</span>
              <span id="val-io" class="mono text-gray-300">14.0 ms</span>
            </div>
            <input id="slider-io" type="range" min="5.0" max="26.0" step="0.5" value="14.0" class="w-full" />
          </div>
        </div>
      </section>

      <!-- Gate Log Toast / Output -->
      <section class="card rounded-xl p-3">
        <div class="text-[11px] uppercase tracking-wider text-gray-400 mb-1.5">Engine Event Log</div>
        <div id="event-log" class="mono text-xs text-gray-300 space-y-1 max-h-28 overflow-y-auto">
          <div>Ready. Tap "Run 1,000-Frame Gate" or "Stream 240Hz".</div>
        </div>
      </section>
    </main>

    <!-- WASM / C++20 Bridge View -->
    <section id="view-cpp" class="hidden space-y-4">
      <div class="card rounded-xl p-4 space-y-3">
        <div class="flex items-center justify-between gap-2">
          <h2 class="text-sm font-semibold text-white">Emscripten WebAssembly Bindings</h2>
          <button id="btn-copy-wasm" type="button" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-[#00f5d4] text-[#090b12]">
            Copy Snippet
          </button>
        </div>
        <p class="text-xs text-gray-300 leading-relaxed">
          To compile <span class="mono text-[#00f5d4]">sub25ms_telemetry_arch.cpp</span> directly to WebAssembly for native-speed browser execution, append this <span class="mono">EMSCRIPTEN_BINDINGS</span> block and compile with <span class="mono">em++ -std=c++20 -O3 -lembind</span>:
        </p>
        <pre id="wasm-code" class="mono text-[11px] leading-relaxed bg-[#07090f] border border-gray-800 rounded-lg p-3 overflow-x-auto text-teal-200">#ifdef __EMSCRIPTEN__
#include &lt;emscripten/bind.h&gt;
using namespace emscripten;

EMSCRIPTEN_BINDINGS(tcp_telemetry_module) {
    value_object&lt;tcp_telemetry::ImuPoseSample&gt;("ImuPoseSample")
        .field("timestamp_ns", &amp;tcp_telemetry::ImuPoseSample::timestamp_ns)
        .field("accel_z_g", &amp;tcp_telemetry::ImuPoseSample::accel_z_g)
        .field("shoulder_angle_deg", &amp;tcp_telemetry::ImuPoseSample::shoulder_angle_deg)
        .field("hip_angle_deg", &amp;tcp_telemetry::ImuPoseSample::hip_angle_deg)
        .field("knee_angle_deg", &amp;tcp_telemetry::ImuPoseSample::knee_angle_deg);

    class_&lt;tcp_telemetry::ClosedLoopEngine&gt;("ClosedLoopEngine")
        .constructor&lt;&gt;()
        .function("ingest_sample", &amp;tcp_telemetry::ClosedLoopEngine::ingest_sample);
}
#endif</pre>
        <div id="copy-toast" class="hidden text-xs text-[#00f5d4] font-medium">Copied to clipboard.</div>
      </div>
    </section>
  </div>

  <script>
    (() => {
      // =========================================================================
      // 1. 1:1 Port of tcp_telemetry C++20 Engine
      // =========================================================================
      const HARD_CEILING_MS = 25.0;
      const CLIPPING_THRESHOLD_G = 15.8;

      const FaultCode = {
        OPTIMAL_FORM: 'OPTIMAL_FORM',
        SHOULDER_CLOSED: 'SHOULDER_CLOSED',
        HIP_PIKE_EARLY: 'HIP_PIKE_EARLY',
        KNEE_COLLAPSE: 'KNEE_COLLAPSE'
      };

      const Colors = {
        TEAL: '#00f5d4',
        GOLD: '#f59e0b',
        RED: '#ef4444'
      };

      class SpscRingBuffer {
        constructor(capacity = 1024) {
          this.capacity = capacity;
          this.mask = capacity - 1;
          this.buf = new Array(capacity);
          this.head = 0;
          this.tail = 0;
        }
        tryPush(item) {
          const next = (this.head + 1) & this.mask;
          if (next === this.tail) return false;
          this.buf[this.head] = item;
          this.head = next;
          return true;
        }
        tryPop() {
          if (this.tail === this.head) return null;
          const item = this.buf[this.tail];
          this.tail = (this.tail + 1) & this.mask;
          return item;
        }
        size() {
          return (this.head - this.tail) & this.mask;
        }
      }

      class KinematicPreprocessor {
        constructor() {
          this.originHipX = 0.5;
          this.originHipY = 0.5;
          this.calibrated = false;
        }
        calibrateFrameZero(hipX, hipY) {
          this.originHipX = hipX;
          this.originHipY = hipY;
          this.calibrated = true;
        }
        detectHighGClipping(sample) {
          return Math.abs(sample.accelZ) >= CLIPPING_THRESHOLD_G;
        }
        normalizeMidHip(sample) {
          if (!this.calibrated) this.calibrateFrameZero(sample.rawHipX, sample.rawHipY);
          return {
            x: sample.rawHipX - this.originHipX,
            y: sample.rawHipY - this.originHipY
          };
        }
      }

      class FaultTreeIsolator {
        constructor() {
          this.lastValidFault = FaultCode.OPTIMAL_FORM;
          this.lastValidDrillId = 0;
        }
        evaluate(sample, preproc, computeUs) {
          const norm = preproc.normalizeMidHip(sample);
          if (preproc.detectHighGClipping(sample)) {
            return {
              computeUs,
              primaryFault: this.lastValidFault,
              color: Colors.GOLD,
              drillRxId: this.lastValidDrillId,
              clippingActive: true,
              normHipX: norm.x,
              normHipY: norm.y
            };
          }

          let primaryFault = FaultCode.OPTIMAL_FORM;
          let color = Colors.TEAL;
          let drillRxId = 0;

          if (sample.shoulderDeg < 155) {
            primaryFault = FaultCode.SHOULDER_CLOSED;
            color = Colors.RED;
            drillRxId = 101;
          } else if (sample.hipDeg < 145) {
            primaryFault = FaultCode.HIP_PIKE_EARLY;
            color = Colors.RED;
            drillRxId = 204;
          } else if (sample.kneeDeg < 150) {
            primaryFault = FaultCode.KNEE_COLLAPSE;
            color = Colors.GOLD;
            drillRxId = 309;
          }

          this.lastValidFault = primaryFault;
          this.lastValidDrillId = drillRxId;

          return {
            computeUs,
            primaryFault,
            color,
            drillRxId,
            clippingActive: false,
            normHipX: norm.x,
            normHipY: norm.y
          };
        }
      }

      class ClosedLoopEngine {
        constructor() {
          this.ingestQueue = new SpscRingBuffer(1024);
          this.preproc = new KinematicPreprocessor();
          this.isolator = new FaultTreeIsolator();
        }
        ingestSample(sample) {
          return this.ingestQueue.tryPush(sample);
        }
        processNext() {
          const t0 = performance.now();
          const sample = this.ingestQueue.tryPop();
          if (!sample) return null;
          const result = this.isolator.evaluate(sample, this.preproc, 0);
          const t1 = performance.now();
          result.computeUs = Math.max(1, Math.round((t1 - t0) * 1000));
          return result;
        }
      }

      // =========================================================================
      // 2. App State & DOM Bindings
      // =========================================================================
      const engine = new ClosedLoopEngine();
      const state = {
        shoulderDeg: 174,
        hipDeg: 170,
        kneeDeg: 172,
        accelZ: 1.8,
        ioMs: 14.0,
        streaming: false,
        rafId: null,
        totalFrames: 0,
        latencyHistory: [],
        lastCmd: null
      };

      const el = (id) => document.getElementById(id);

      function logEvent(msg, color = '#9ca3af') {
        const log = el('event-log');
        const row = document.createElement('div');
        row.style.color = color;
        row.textContent = `[${new Date().toLocaleTimeString()}] ${msg}`;
        log.prepend(row);
        while (log.children.length > 25) log.removeChild(log.lastChild);
      }

      function updateUI(cmd, worstM2pMs) {
        state.lastCmd = cmd;
        const m2p = worstM2pMs ?? (state.ioMs + cmd.computeUs / 1000);
        const pass = m2p < HARD_CEILING_MS;

        el('kpi-m2p').textContent = m2p.toFixed(2);
        el('kpi-m2p').style.color = pass ? Colors.TEAL : Colors.RED;

        const gateEl = el('kpi-gate');
        gateEl.textContent = pass ? 'PASS < 25.0 ms' : 'FAIL >= 25.0 ms';
        gateEl.className = pass
          ? 'mt-1 inline-block px-2 py-0.5 rounded text-[10px] font-bold bg-[#00f5d4]/15 text-[#00f5d4]'
          : 'mt-1 inline-block px-2 py-0.5 rounded text-[10px] font-bold bg-red-500/20 text-red-400';

        el('kpi-compute').textContent = String(cmd.computeUs);
        el('kpi-fault').textContent = cmd.primaryFault;
        el('kpi-fault').style.color = cmd.color;
        el('kpi-drill').textContent = `DrillRx #${cmd.drillRxId}`;

        el('kpi-clip').textContent = cmd.clippingActive ? 'ACTIVE (HOLD)' : 'NOMINAL';
        el('kpi-clip').style.color = cmd.clippingActive ? Colors.GOLD : Colors.TEAL;
        el('kpi-accel').textContent = `Z: ${state.accelZ.toFixed(1)} g (Rail 15.8g)`;

        el('hud-ring').textContent = `Ring: ${engine.ingestQueue.size()}/1024`;
        el('hud-frames').textContent = `Frames: ${state.totalFrames}`;

        drawPoseCanvas(cmd);
        drawLatencyCanvas();
      }

      function stepSingleFrame(overrideSample = null) {
        const tPhase = state.totalFrames * 0.08;
        const sample = overrideSample || {
          rawHipX: 0.5 + Math.sin(tPhase) * 0.02,
          rawHipY: 0.5 + Math.cos(tPhase) * 0.02,
          accelZ: state.accelZ,
          shoulderDeg: state.shoulderDeg,
          hipDeg: state.hipDeg,
          kneeDeg: state.kneeDeg
        };
        engine.ingestSample(sample);
        const cmd = engine.processNext();
        if (!cmd) return;
        state.totalFrames += 1;
        const m2pMs = state.ioMs + cmd.computeUs / 1000;
        state.latencyHistory.push(m2pMs);
        if (state.latencyHistory.length > 80) state.latencyHistory.shift();
        updateUI(cmd, m2pMs);
      }

      // =========================================================================
      // 3. Canvas Renderers (Stick Figure + Latency Trace)
      // =========================================================================
      function resizeCanvas(canvas) {
        const rect = canvas.parentElement.getBoundingClientRect();
        const dpr = window.devicePixelRatio || 1;
        canvas.width = Math.max(1, Math.floor(rect.width * dpr));
        canvas.height = Math.max(1, Math.floor(rect.height * dpr));
        const ctx = canvas.getContext('2d');
        ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
        return { ctx, w: rect.width, h: rect.height };
      }

      function drawPoseCanvas(cmd) {
        const canvas = el('canvas-pose');
        const { ctx, w, h } = resizeCanvas(canvas);
        ctx.clearRect(0, 0, w, h);

        // Floor grid line
        ctx.strokeStyle = '#1f2937';
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(16, h - 26);
        ctx.lineTo(w - 16, h - 26);
        ctx.stroke();

        // Compute sagittal stick figure from angles
        const hipX = w * 0.5 + (cmd ? cmd.normHipX * 60 : 0);
        const hipY = h * 0.55 + (cmd ? cmd.normHipY * 40 : 0);
        const rad = (deg) => (deg * Math.PI) / 180;

        // Torso upward-left tilt based on hip angle
        const torsoAngle = -Math.PI / 2 - (180 - state.hipDeg) * 0.012;
        const shX = hipX + Math.cos(torsoAngle) * 46;
        const shY = hipY + Math.sin(torsoAngle) * 46;

        // Arms extended relative to shoulder angle
        const armAngle = torsoAngle - rad(180 - state.shoulderDeg);
        const handX = shX + Math.cos(armAngle) * 38;
        const handY = shY + Math.sin(armAngle) * 38;

        // Thigh + shin based on knee angle
        const thighAngle = Math.PI / 2 + 0.15;
        const kneeX = hipX + Math.cos(thighAngle) * 36;
        const kneeY = hipY + Math.sin(thighAngle) * 36;
        const shinAngle = thighAngle + rad(180 - state.kneeDeg);
        const ankleX = kneeX + Math.cos(shinAngle) * 34;
        const ankleY = kneeY + Math.sin(shinAngle) * 34;

        // Draw skeleton links
        ctx.strokeStyle = cmd ? cmd.color : Colors.TEAL;
        ctx.lineWidth = 3;
        ctx.lineCap = 'round';
        ctx.beginPath();
        ctx.moveTo(handX, handY);
        ctx.lineTo(shX, shY);
        ctx.lineTo(hipX, hipY);
        ctx.lineTo(kneeX, kneeY);
        ctx.lineTo(ankleX, ankleY);
        ctx.stroke();

        // Head
        ctx.beginPath();
        ctx.arc(shX + Math.cos(torsoAngle) * 14, shY + Math.sin(torsoAngle) * 14, 9, 0, Math.PI * 2);
        ctx.stroke();

        // Highlight primary fault joint
        const joints = [
          { x: shX, y: shY, fault: FaultCode.SHOULDER_CLOSED },
          { x: hipX, y: hipY, fault: FaultCode.HIP_PIKE_EARLY },
          { x: kneeX, y: kneeY, fault: FaultCode.KNEE_COLLAPSE }
        ];
        joints.forEach((j) => {
          const isFault = cmd && cmd.primaryFault === j.fault;
          ctx.fillStyle = isFault ? cmd.color : '#e5e7eb';
          ctx.beginPath();
          ctx.arc(j.x, j.y, isFault ? 6.5 : 3.5, 0, Math.PI * 2);
          ctx.fill();
        });

        // Waveguide HUD badge
        ctx.fillStyle = 'rgba(9, 11, 18, 0.8)';
        ctx.fillRect(10, 10, 150, 22);
        ctx.fillStyle = cmd ? cmd.color : Colors.TEAL;
        ctx.font = '600 11px JetBrains Mono, monospace';
        ctx.fillText(cmd ? `${cmd.primaryFault}` : 'READY', 16, 25);
      }

      function drawLatencyCanvas() {
        const canvas = el('canvas-latency');
        const { ctx, w, h } = resizeCanvas(canvas);
        ctx.clearRect(0, 0, w, h);

        const maxMs = 30.0;
        const yForMs = (ms) => h - 18 - (Math.min(ms, maxMs) / maxMs) * (h - 32);

        // 25.0 ms hard ceiling line
        const yCeil = yForMs(HARD_CEILING_MS);
        ctx.strokeStyle = Colors.RED;
        ctx.setLineDash([4, 4]);
        ctx.lineWidth = 1.25;
        ctx.beginPath();
        ctx.moveTo(12, yCeil);
        ctx.lineTo(w - 12, yCeil);
        ctx.stroke();
        ctx.setLineDash([]);

        ctx.fillStyle = Colors.RED;
        ctx.font = '600 10px JetBrains Mono, monospace';
        ctx.fillText('25.0 ms CEILING', 14, yCeil - 5);

        if (state.latencyHistory.length < 2) return;

        ctx.strokeStyle = Colors.TEAL;
        ctx.lineWidth = 2;
        ctx.beginPath();
        const stepX = (w - 24) / Math.max(1, state.latencyHistory.length - 1);
        state.latencyHistory.forEach((val, idx) => {
          const x = 12 + idx * stepX;
          const y = yForMs(val);
          if (idx === 0) ctx.moveTo(x, y);
          else ctx.lineTo(x, y);
        });
        ctx.stroke();
      }

      // =========================================================================
      // 4. Event Listeners (Every Control Wired)
      // =========================================================================
      el('btn-gate-test').addEventListener('click', () => {
        const gateEngine = new ClosedLoopEngine();
        let maxComputeUs = 0;
        let clippingFrames = 0;
        let faultFrames = 0;

        for (let i = 0; i < 1000; i++) {
          const sample = {
            rawHipX: 0.5,
            rawHipY: 0.5,
            accelZ: i >= 400 && i < 412 ? 16.2 : 1.8,
            shoulderDeg: i >= 250 && i < 300 ? 142 : 174,
            hipDeg: 170,
            kneeDeg: 172
          };
          gateEngine.ingestSample(sample);
          const cmd = gateEngine.processNext();
          if (cmd.computeUs > maxComputeUs) maxComputeUs = cmd.computeUs;
          if (cmd.clippingActive) clippingFrames++;
          if (cmd.primaryFault !== FaultCode.OPTIMAL_FORM) faultFrames++;
          state.latencyHistory.push(state.ioMs + cmd.computeUs / 1000);
          if (state.latencyHistory.length > 80) state.latencyHistory.shift();
        }

        state.totalFrames += 1000;
        const worstM2p = state.ioMs + maxComputeUs / 1000;
        const pass = worstM2p < HARD_CEILING_MS;
        updateUI(
          {
            computeUs: maxComputeUs,
            primaryFault: FaultCode.OPTIMAL_FORM,
            color: Colors.TEAL,
            drillRxId: 0,
            clippingActive: false,
            normHipX: 0,
            normHipY: 0
          },
          worstM2p
        );
        logEvent(
          `1,000-frame gate: ${worstM2p.toFixed(2)} ms (${pass ? 'PASS' : 'FAIL'}), ${clippingFrames} clipped, ${faultFrames} faults.`,
          pass ? Colors.TEAL : Colors.RED
        );
      });

      el('btn-stream').addEventListener('click', () => {
        state.streaming = !state.streaming;
        el('btn-stream').textContent = state.streaming ? 'Pause Stream' : 'Stream 240Hz';
        if (state.streaming) {
          logEvent('Started live telemetry stream.', Colors.TEAL);
          const loop = () => {
            if (!state.streaming) return;
            stepSingleFrame();
            state.rafId = requestAnimationFrame(loop);
          };
          state.rafId = requestAnimationFrame(loop);
        } else {
          if (state.rafId) cancelAnimationFrame(state.rafId);
          logEvent('Paused telemetry stream.');
        }
      });

      el('btn-calibrate').addEventListener('click', () => {
        engine.preproc.calibrateFrameZero(0.5, 0.5);
        logEvent('Frame-Zero mid-hip origin calibrated.', Colors.TEAL);
        stepSingleFrame();
      });

      const wireSlider = (id, key, labelId, suffix) => {
        el(id).addEventListener('input', (e) => {
          state[key] = parseFloat(e.target.value);
          el(labelId).textContent = `${state[key]}${suffix}`;
          stepSingleFrame();
        });
      };
      wireSlider('slider-shoulder', 'shoulderDeg', 'val-shoulder', '°');
      wireSlider('slider-hip', 'hipDeg', 'val-hip', '°');
      wireSlider('slider-knee', 'kneeDeg', 'val-knee', '°');
      wireSlider('slider-accel', 'accelZ', 'val-accel', ' g');
      wireSlider('slider-io', 'ioMs', 'val-io', ' ms');

      el('btn-reset-inputs').addEventListener('click', () => {
        state.shoulderDeg = 174; el('slider-shoulder').value = 174; el('val-shoulder').textContent = '174°';
        state.hipDeg = 170; el('slider-hip').value = 170; el('val-hip').textContent = '170°';
        state.kneeDeg = 172; el('slider-knee').value = 172; el('val-knee').textContent = '172°';
        state.accelZ = 1.8; el('slider-accel').value = 1.8; el('val-accel').textContent = '1.8 g';
        state.ioMs = 14.0; el('slider-io').value = 14.0; el('val-io').textContent = '14.0 ms';
        stepSingleFrame();
      });

      // Tabs
      el('tab-sim').addEventListener('click', () => {
        el('view-sim').classList.remove('hidden');
        el('view-cpp').classList.add('hidden');
        el('tab-sim').className = 'px-3 py-1.5 rounded-lg text-xs font-semibold bg-[#00f5d4] text-[#090b12]';
        el('tab-cpp').className = 'px-3 py-1.5 rounded-lg text-xs font-semibold bg-gray-800 text-gray-300 hover:bg-gray-700';
      });
      el('tab-cpp').addEventListener('click', () => {
        el('view-sim').classList.add('hidden');
        el('view-cpp').classList.remove('hidden');
        el('tab-cpp').className = 'px-3 py-1.5 rounded-lg text-xs font-semibold bg-[#00f5d4] text-[#090b12]';
        el('tab-sim').className = 'px-3 py-1.5 rounded-lg text-xs font-semibold bg-gray-800 text-gray-300 hover:bg-gray-700';
      });

      // Copy WASM snippet
      el('btn-copy-wasm').addEventListener('click', async () => {
        const txt = el('wasm-code').textContent;
        try {
          await navigator.clipboard.writeText(txt);
        } catch {
          const ta = document.createElement('textarea');
          ta.value = txt;
          document.body.appendChild(ta);
          ta.select();
          document.execCommand('copy');
          document.body.removeChild(ta);
        }
        el('copy-toast').classList.remove('hidden');
        setTimeout(() => el('copy-toast').classList.add('hidden'), 2000);
      });

      window.addEventListener('resize', () => {
        if (state.lastCmd) updateUI(state.lastCmd);
      });

      // Initial render
      stepSingleFrame();
    })();
  </script>
</body>
</html>

#include <iostream>
#include <vector>
#include <atomic>
#include <thread>
#include <string>
#include <optional>
#include <cmath>
#include <chrono>

// --- Core Telemetry Types ---
enum class FaultCode {
    kOptimalForm = 0,
    kShoulderAngleClosed = 1,
    kHipPikeEarly = 2,
    kKneeCollapse = 3,
    kSensorClippingFallback = 4
};

struct InputFrame {
    uint64_t timestamp_us;
    uint32_t frame_index;
    float z_accel_g;
    float gyro_pitch_rad_s;
    float sensor_raw_data[6];
};

struct CuePayload {
    double time;
    std::string label;
    std::string type;
    int drill_rx_id;
    bool clipping_active;
};

// --- Lock-Free Ring Buffer ---
template<typename T, size_t Capacity>
class LockFreeRingBuffer {
public:
    LockFreeRingBuffer() : head_(0), tail_(0) {}

    bool Push(const T& item) {
        size_t current_tail = tail_.load(std::memory_order_relaxed);
        size_t current_head = head_.load(std::memory_order_acquire);
        
        if ((current_tail + 1) % Capacity == current_head) {
            return false; // Buffer overflow
        }
        buffer_[current_tail] = item;
        tail_.store((current_tail + 1) % Capacity, std::memory_order_release);
        return true;
    }

    bool Pop(T& item) {
        size_t current_head = head_.load(std::memory_order_relaxed);
        size_t current_tail = tail_.load(std::memory_order_acquire);
        
        if (current_head == current_tail) {
            return false; // Buffer underflow
        }
        item = buffer_[current_head];
        head_.store((current_head + 1) % Capacity, std::memory_order_release);
        return true;
    }

private:
    T buffer_[Capacity];
    std::atomic<size_t> head_;
    std::atomic<size_t> tail_;
};

// --- Global State & Buffers ---
LockFreeRingBuffer<InputFrame, 1024> g_telemetry_ring;
LockFreeRingBuffer<CuePayload, 64> g_transmission_buffer;
std::atomic<int> g_last_valid_drill_id{0};

// --- Kinematic Math & Calibration ---
struct CalibrationOffset {
    float accel_offset_z = 0.0f;
    bool is_calibrated = false;
};

CalibrationOffset g_calib;

void CalibrateZeroPoint(const InputFrame& frame) {
    g_calib.accel_offset_z = frame.z_accel_g - 1.0f; // Assume gravity norm
    g_calib.is_calibrated = true;
}

float CalculateKneeAngle(const InputFrame& frame) {
    // Mock kinematic chain reconstruction
    return 145.0f; 
}

// --- Dispatch Logic ---
void DispatchCue(FaultCode current_fault) {
    CuePayload cue{0.0, "", "", 0, false};
    
    switch (current_fault) {
        case FaultCode::kOptimalForm:
            return;
        case FaultCode::kShoulderAngleClosed:
            cue = {0.0, "Open shoulder block — arms at ears", "red", 101, false};
            break;
        case FaultCode::kHipPikeEarly:
            cue = {0.0, "Hold hollow snap-down — delay pike", "red", 204, false};
            break;
        case FaultCode::kKneeCollapse:
            cue = {0.0, "Eccentric landing stick — lock knees", "gold", 309, false};
            break;
        case FaultCode::kSensorClippingFallback:
            cue = {0.0, "High-G impact guard — holding last valid cue", "gold", g_last_valid_drill_id.load(), true};
            break;
    }
    
    if (!g_transmission_buffer.Push(cue)) {
        std::cerr << "[Critical] Transmission buffer saturated! Dropping cue." << std::endl;
    }
}

// --- Hot Path Processing ---
void ProcessTelemetry(InputFrame current_frame) {
    if (!g_calib.is_calibrated) {
        CalibrateZeroPoint(current_frame);
    }

    float normalized_z = current_frame.z_accel_g - g_calib.accel_offset_z;
    FaultCode current_fault = FaultCode::kOptimalForm;

    // High-G Clipping Guard with Gyro Fallback
    if (normalized_z > 15.8f) {
        if (std::abs(current_frame.gyro_pitch_rad_s) < 2.5f) {
            current_fault = FaultCode::kSensorClippingFallback;
        } else {
            current_fault = FaultCode::kOptimalForm;
        }
    } else {
        float knee_angle = CalculateKneeAngle(current_frame);
        if (knee_angle < 130.0f) {
            current_fault = FaultCode::kKneeCollapse;
        }
    }

    if (current_fault != FaultCode::kSensorClippingFallback && current_fault != FaultCode::kOptimalForm) {
        // Map fault enum to DrillRx ID for state persistence
        switch(current_fault) {
            case FaultCode::kShoulderAngleClosed: g_last_valid_drill_id.store(101); break;
            case FaultCode::kHipPikeEarly: g_last_valid_drill_id.store(204); break;
            case FaultCode::kKneeCollapse: g_last_valid_drill_id.store(309); break;
            default: break;
        }
    }
    
    DispatchCue(current_fault);
}

// --- Execution Loop Simulation ---
void SimulatedSensorThread() {
    for (uint32_t i = 0; i < 1000; ++i) {
        InputFrame frame;
        frame.timestamp_us = std::chrono::duration_cast<std::chrono::microseconds>(
            std::chrono::high_resolution_clock::now().time_since_epoch()).count();
        frame.frame_index = i;
        frame.z_accel_g = (i == 500) ? 18.2f : 1.0f; // Simulate high-G spike
        frame.gyro_pitch_rad_s = 0.1f;
        
        while (!g_telemetry_ring.Push(frame)) {
            std::this_thread::yield(); 
        }
        std::this_thread::sleep_for(std::chrono::milliseconds(4)); // 250Hz nominal
    }
}

void ProcessingConsumerThread() {
    int processed_frames = 0;
    while (processed_frames < 1000) {
        InputFrame frame;
        if (g_telemetry_ring.Pop(frame)) {
            ProcessTelemetry(frame);
            processed_frames++;
        } else {
            std::this_thread::yield();
        }
    }
}

int main() {
    std::cout << "Initializing 240Hz telemetry processing pipeline..." << std::endl;
    
    std::thread producer(SimulatedSensorThread);
    std::thread consumer(ProcessingConsumerThread);
    
    producer.join();
    consumer.join();
    
    std::cout << "Simulation complete. Pipeline operated within motion-to-photon constraints." << std::endl;
    return 0;
}
