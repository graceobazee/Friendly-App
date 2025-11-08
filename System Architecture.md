# Friendly System Architecture

**Technical Overview:** Friendly is a mobile application designed to help users maintain meaningful connections. It leverages a serverless backend with Firebase Functions, real-time notifications via Firebase Cloud Messaging, and Cloud Firestore for data storage to deliver a seamless, scalable, and privacy-first relationship management experience.

---

## 1. Introduction & Goals

**Goal:** Make it effortless for users to maintain intentional connections with friends, family, and mentors.

**Scope:** Mobile application only, cross-platform (iOS & Android), cloud-backed with serverless architecture.

---

## 2. Constraints

- Mobile-only (iOS & Android)
- User privacy-first approach
- Must integrate with device contacts
- Lightweight storage requirements
- Offline resilience for reminders and data caching
- Serverless backend architecture (no traditional server management)

---

## 3. Solution Strategy

- Use Firebase serverless architecture (Firebase Functions) to reduce infrastructure overhead and enable automatic scaling.
- Flutter for cross-platform deployment with native performance and single codebase.
- Push notifications via Firebase Cloud Messaging (FCM) for reminders and prompts.
- Cloud Firestore for real-time, scalable NoSQL data storage.
- Privacy-centric design for all data with Firestore security rules.
- **Feasibility:** Serverless backend ensures scalability and minimal operational overhead; frontend is maintainable with Flutter and modular components.

---

## 4. Building Block View

**Frontend Components:**
- **Framework:** Flutter
- **Libraries:** State management (Provider/Riverpod/Bloc), push notification handler (firebase_messaging)
- **Screens:** Connection tiers, Reminders, Message prompts, Moments feed, Visual insights
- **Local Storage:** SQLite/Hive for offline caching

**Backend Components:**
- **Platform:** Firebase (serverless)
- **Language:** Node.js (JavaScript/TypeScript)
- **Firebase Functions:** 
  - Reminder scheduler
  - Smart suggestion engine
  - Notification dispatcher
  - Authentication handler
  - Data processing and analytics

**Database:** 
- **Cloud Firestore** collections:
  - Users
  - Contacts
  - Reminders
  - Moments
  - Insights

**Authentication:**
- Firebase Authentication (Email/Password, Google, Apple Sign-In)

**Communication:**
- Frontend → Backend via HTTPS callable Firebase Functions
- Backend → Firestore for reading/writing data
- Backend → Frontend via FCM push notifications
- Frontend reads local cache for offline access

---

## 5. Runtime View

### Reminder Flow
1. Cloud Scheduler triggers Firebase Function at scheduled intervals
2. Function queries Firestore for due reminders
3. Function sends push notification via FCM to user's device
4. User receives notification and takes action (opens app, sends message)
5. Frontend updates Firestore with interaction data
6. Function updates reminder status and schedules next reminder

### Smart Suggestions Flow
1. Firebase Function analyzes user interaction patterns from Firestore
2. Function calculates suggestion scores based on recent activity
3. Function writes suggestions to Firestore
4. Frontend listens to Firestore updates in real-time
5. Push notification dispatched for high-priority suggestions
6. Frontend displays suggestions with context

### Moments Sharing Flow
1. User creates moment in Flutter app
2. Frontend uploads media to Firebase Storage (if applicable)
3. Frontend writes moment data to Firestore
4. Firestore triggers Firebase Function for processing
5. Function updates relevant user feeds
6. Real-time listeners update connected clients

---

## 6. Deployment View

**Mobile Application:**
- iOS: Deployed via Apple App Store
- Android: Deployed via Google Play Store
- Build system: Flutter build tools
- CI/CD: GitHub Actions for automated builds and releases

**Backend (Serverless):**
- Hosted on: Google Cloud Platform (via Firebase)
- Firebase Functions: Automatically deployed and scaled
- Cloud Firestore: Managed NoSQL database
- Firebase Storage: Media file storage
- Firebase Cloud Messaging: Push notification service

**Infrastructure:**
- No traditional servers required
- Automatic scaling based on demand
- Global CDN for static assets
- Multi-region data replication for Firestore

---

## 7. Cross-cutting Concepts

### Privacy & Security
- Privacy-first design philosophy
- Opt-in notifications and data sharing
- Firestore security rules enforce data access policies
- All communication over HTTPS
- User data encrypted at rest and in transit

### Offline Capability
- Local caching with SQLite/Hive for Flutter
- Firestore offline persistence enabled
- Reminders work offline with local scheduling
- Sync when connection restored

### Performance
- Lazy loading for large contact lists
- Pagination for Moments feed
- Image compression before upload
- Optimized Firestore queries with indexes

### Monitoring & Analytics
- Firebase Analytics for usage tracking
- Firebase Crashlytics for error reporting
- Cloud Functions logging for backend monitoring
- Performance monitoring for app responsiveness

---

## 8. Architectural Decisions (ADR)

| Decision | Description | Rationale |
|----------|------------|-----------|
| Serverless Backend | Use Firebase Functions instead of traditional servers | Reduces infrastructure overhead, automatic scaling, pay-per-use pricing, faster development |
| Cross-platform App | Flutter framework | Enables iOS & Android deployment with single codebase, native performance, hot reload for rapid development |
| Push Notifications | Firebase Cloud Messaging (FCM) | Provides reliable cloud messaging service, integrated with Firebase ecosystem, free tier available |
| NoSQL Database | Cloud Firestore | Real-time synchronization, offline support, automatic scaling, security rules, seamless Firebase integration |
| Privacy-first Data | Firestore security rules & opt-in features | Ensures user data is secure and controlled, builds user trust, compliance-ready |
| No Backend Framework | Direct Firebase Functions (no Express) | Reduces complexity, faster cold starts, better integration with Firebase services |

---

## 9. Quality Attributes

### Scalability
- Can handle millions of users without infrastructure changes
- Firebase Functions scale automatically based on demand
- Firestore handles high read/write throughput

### Performance
- Push notifications delivered in <1 second
- App startup time <2 seconds
- Real-time updates appear within 500ms
- Offline-first architecture ensures responsive UI

### Security
- End-to-end encryption for data in transit
- Firestore security rules prevent unauthorized access
- No auto-posting or data sharing without explicit consent
- Regular security audits and updates

### Availability
- Firebase SLA: 99.95% uptime
- Multi-region data replication
- Automatic failover and recovery
- No single point of failure

### Usability
- Minimal, intuitive mobile UI
- Onboarding flow <2 minutes
- Natural language for reminders
- Contextual help and tooltips

### Maintainability
- Single codebase for iOS and Android
- Serverless architecture reduces DevOps overhead
- Modular Flutter component architecture
- Clear separation of concerns

---

## 10. Risks & Technical Debt

### Current Risks
- **Vendor Lock-in:** Heavy dependency on Firebase may limit future backend flexibility
  - *Mitigation:* Abstract Firebase calls behind service layer for easier migration
- **Real-time Updates:** Could increase data usage on mobile networks
  - *Mitigation:* Implement smart sync strategies, batch updates, user controls
- **Cold Starts:** Firebase Functions may have latency on first invocation
  - *Mitigation:* Use minimum instances for critical functions, optimize function code
- **AI Smart Suggestions:** May need refinement based on user patterns and feedback
  - *Mitigation:* Start with simple heuristics, iterate based on data
- **Cost Scaling:** Firebase costs could increase significantly with user growth
  - *Mitigation:* Monitor usage, optimize queries, implement caching strategies

### Technical Debt
- Need to implement comprehensive error handling across all Firebase Functions
- Message suggestion templates need localization for international users
- Analytics implementation should be standardized across all screens
- Need automated testing suite for Firebase Functions

---

## 11. Glossary

- **FCM:** Firebase Cloud Messaging - Google's solution for push notifications
- **MVP:** Minimum Viable Product - initial version with core features
- **ADR:** Architectural Decision Record - documentation of key technical decisions
- **Firestore:** Cloud Firestore - Google's NoSQL document database
- **Firebase Functions:** Serverless compute platform for running backend code
- **Flutter:** Google's UI toolkit for building natively compiled applications
- **Cold Start:** Initial latency when serverless function is invoked after being idle
- **Serverless:** Cloud computing model where infrastructure is fully managed by provider

---

*Friendly: a quiet, intelligent companion for meaningful human connections.*