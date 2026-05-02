# Election Process Education System

A comprehensive web application designed to educate citizens about the Indian election process. Built for the Google PromptWars competition, this project focuses on accessibility, performance, and utilizing Firebase services effectively.

## 🏆 Problem Statement Alignment
This project directly addresses the need for civic education by providing:
- Step-by-step guides on how to vote.
- Candidate awareness profiles.
- A secure, simulated EVM (Electronic Voting Machine) experience.
- An interactive quiz to test election knowledge.
- An AI-powered chatbot for quick query resolution.

## ✨ Features
* **Google Authentication**: Secure login using Firebase Auth.
* **Simulated Voting**: Real-time voting simulation backed by Firestore.
* **Anti-Duplicate Voting**: Strict logic ensuring `1 User = 1 Vote`.
* **Live Vote Counting**: Real-time updates of election results using Firestore listeners.
* **AI Chatbot**: Intelligent assistant for election FAQs.
* **Knowledge Quiz**: Interactive assessment of civic knowledge.
* **Admin Dashboard**: Real-time metrics overview (Demo mode).
* **Responsive & Accessible**: Fully usable across devices, following WCAG guidelines (Semantic HTML, ARIA labels, color contrast).

## 🛠️ Technology Stack
* **Frontend**: React (Vite), Tailwind CSS, Framer Motion, Lucide React.
* **Backend & Services**: 
  * Firebase Authentication (Google Provider)
  * Firebase Cloud Firestore (Real-time DB)
  * Firebase Analytics (Event tracking)

## 🚀 Google Services Used
1. **Firebase Authentication**: For seamless Google Sign-in.
2. **Firebase Cloud Firestore**: For storing votes securely and fetching real-time counts.
3. **Firebase Analytics**: For tracking user engagement (e.g., votes cast, quizzes completed).

## 🔒 Security & Anti-Duplication
* The voting logic checks Firestore for existing votes matching the `user.uid`.
* If a vote exists, the UI immediately blocks further attempts.
* **Firestore Security Rules (Recommended Setup):**
  ```javascript
  rules_version = '2';
  service cloud.firestore {
    match /databases/{database}/documents {
      match /votes/{voteId} {
        // Anyone can read total votes
        allow read: if true;
        
        // Only authenticated users can write, and they can only write their own uid
        // They can only create a document if they haven't voted before (handled via queries)
        allow create: if request.auth != null && request.resource.data.userId == request.auth.uid;
        allow update, delete: if false; // Votes cannot be changed or deleted
      }
    }
  }
  ```

## 🧪 Testing Cases Covered
1. **Unauthenticated Access**: Users cannot access the `/vote` route without logging in (redirected to home).
2. **Duplicate Voting**: Attempting to vote twice throws a specific error message.
3. **Network Failure State**: Loading indicators show while casting votes; error messages show on failure.
4. **Mobile Responsiveness**: Navigation menu collapses into a hamburger menu; grid layouts stack vertically on small screens.
5. **Accessibility**: All buttons have clear labels, high contrast text is used, and focus states are visible.

## 💻 Local Setup & Deployment

### Prerequisites
- Node.js (v18+)
- A Firebase Project with Authentication (Google) and Firestore enabled.

### Steps
1. Clone the repository.
2. Run `npm install` to install dependencies.
3. Create a `.env` file (or update `src/firebase.js` directly) with your Firebase config:
   ```env
   VITE_FIREBASE_API_KEY=your_api_key
   VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
   VITE_FIREBASE_PROJECT_ID=your_project_id
   VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
   VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
   VITE_FIREBASE_APP_ID=your_app_id
   VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id
   ```
4. Run `npm run dev` to start the development server.
5. To build for production, run `npm run build`.

## 🎨 Design Decisions
- **Color Palette**: Used trustworthy blues, clean whites, and subtle accents matching Indian thematic colors (Orange/Saffron, Green, Blue).
- **Typography**: Inter font for high legibility across devices.
- **Micro-animations**: Used Framer Motion for smooth transitions, enhancing the premium feel without impacting performance.
