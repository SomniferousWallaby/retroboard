# Agile Retrospective Board 🚀

A real-time, single-page web application for conducting Agile sprint retrospectives. This tool allows teams to collaboratively post notes, discuss topics, and track action items. It's built with vanilla JavaScript and powered by Firebase for backend services, all styled with Tailwind CSS.

-----

## Features ✨

  * **Real-time Collaboration**: Changes are reflected instantly for all users via Firebase Firestore.
  * **User Authentication**: Secure sign-in using Google Authentication.
  * **Multiple Boards**: Create separate boards for different sprints or teams.
  * **Standard Columns**: Includes "Positives," "Negatives," "Discussion Topics," "Action Items," and "Shoutouts."
  * **Anonymity & Reveal**: Cards are initially blurred and author names are hidden. The board creator or an admin can reveal all cards for discussion.
  * **Voting System**: Users can vote on cards to help prioritize topics.
  * **Action Item Carry-over**: Incomplete action items from the most recent board are automatically carried over to a new one.
  * **Centralized Action Item Tracking**: A dedicated page lists all action items from every board, separated into incomplete and completed sections.
  * **Admin Controls**: Admins and board creators can delete boards and any cards.

-----

## Tech Stack 🛠️

  * **Frontend**: HTML5, Vanilla JavaScript (ES Modules), [Tailwind CSS](https://tailwindcss.com/)
  * **Backend**: [Firebase](https://firebase.google.com/)
      * **Authentication**: For user sign-in.
      * **Firestore**: As the real-time NoSQL database.

-----

## Getting Started

To run this project, you'll need a Firebase project and a Firestore database

### Setup Instructions

1.  **Clone or Download the Repository**: Get the `index.html` file onto your local machine.

2.  **Create a Firebase Project**:

      * Go to the [Firebase Console](https://console.firebase.google.com/).
      * Click "Add project" and follow the on-screen instructions.
      * Once the project is created, add a **Web App** to it.
      * Firebase will provide you with a configuration object. **Copy this object.**

3.  **Configure Firebase**:

      * In the same directory as `index.html`, replace the content of the file named `firebaseConf.json`.
      * Paste the configuration object you copied from Firebase into this file. It should look like this:

    <!-- end list -->

    ```json
    {
      "apiKey": "AIza....",
      "authDomain": "your-project-id.firebaseapp.com",
      "projectId": "your-project-id",
      "storageBucket": "your-project-id.appspot.com",
      "messagingSenderId": "...",
      "appId": "1:..."
    }
    ```

4.  **Enable Firebase Services**:

      * In your Firebase project console, go to the **Authentication** section. Click the "Sign-in method" tab and enable **Google** as a provider.
      * Go to the **Firestore Database** section. Click "Create database" and start in **test mode** for easy setup.

5.  **Run the Application**:

      * Download and install the Firebase CLI
      * Authenticate to Firebase using an account with sufficient permissions to your Firebase project
      * In the same directory as this code, run `firebase deploy`

-----

## Firestore Data Structure

The application uses the following data structure in Firestore:

```
/
├── admins/{userId}
│   └── (doc) { isAdmin: true }
│
└── artifacts/retro-board-app/
    ├── public/data/retro_boards/{boardId}
    │   ├── (doc) { name, createdBy, creatorId, createdAt, cardsRevealed }
    │   └── items/{itemId}
    │       └── (doc) { text, column, userId, authorName, votes, completed, ... }
    │
    └── users/{userId}/profile/
        └── data
            └── (doc) { displayName, email, isAdmin }
```

### Admin Role

To grant a user admin privileges:

1.  Find the user's UID from the Firebase Authentication tab.
2.  In Firestore, manually create a new document in the `admins` collection.
3.  Set the **Document ID** to the user's UID.
4.  Add a boolean field `isAdmin` and set it to `true`.
