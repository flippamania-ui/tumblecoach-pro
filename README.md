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
