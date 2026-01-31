# Frontend Documentation

This document provides an overview of the ZKredit frontend application, its architecture, and key components.

## Overview

The frontend is a React application built with Vite that serves as the primary user interface for the ZKredit demo. It guides the user through a 5-step loan application process, demonstrating the core concepts of privacy-preserving credit scoring using ZK-SNARKs.

The application is designed to be a self-contained demonstration, with a focus on visualizing the security and privacy features of the ZKredit protocol.

## Tech Stack

-   **Framework:** React 18
-   **Build Tool:** Vite
-   **Styling:** Vanilla CSS with a custom glassmorphism design system
-   **API Communication:** `fetch` API

## Project Structure

The frontend code is located in the `client/` directory.

```
client/
├── public/
│   └── vite.svg
├── src/
│   ├── assets/
│   │   └── react.svg
│   ├── App.css
│   ├── App.jsx         # Main application component
│   ├── index.css
│   └── main.jsx        # React entry point
├── .gitignore
├── index.html
├── package.json
└── vite.config.js
```

-   **`main.jsx`**: The entry point of the React application.
-   **`App.jsx`**: The main and only component that contains the entire application logic and UI.
-   **`index.css` & `App.css`**: Contains the styling for the application, implementing a glassmorphism design.

## Architecture

The frontend is architected as a single-page application (SPA) with a single, large component (`App.jsx`). This component manages the state and flow of the entire application.

### Component-Based Structure

Although contained within a single file, the UI is logically divided into several functional components, each responsible for a specific part of the user interface or a step in the application flow:

-   **`Header`**: Displays the application title and the network it's connected to.
-   **`ProgressStepper`**: A visual indicator of the user's progress through the 5 steps of the loan application.
-   **`UserSelection`**: Allows the user to choose from different predefined user profiles, including regular users and attackers, to simulate various scenarios.
-   **`Step1_BankData`**: Handles the fetching of financial data from the mock bank oracle.
-   **`Step2_zkTLS`**: Simulates the process of zkTLS for data provenance, showing the TLS handshake and the comparison between ECDSA and zkTLS.
-   **`Step3_MLScore`**: Simulates the local execution of the machine learning model to score the user's creditworthiness.
-   **`Step4_ZKProof`**: Simulates the generation of the ZK-SNARK proof.
-   **`Step5_Verification`**: Visualizes the 5-layer on-chain verification process, showing each security check.
-   **`FinalResult`**: Displays the final outcome of the loan application (approved or rejected).
-   **`AttackFailedResult`**: Shown when one of the security layers detects and blocks an attack.

### State Management

The application uses React's built-in `useState` hook for all state management. The main `App` component holds the global state of the application, such as:

-   `currentStep`: The current step the user is on.
-   `completedSteps`: An array of completed step IDs.
-   `selectedUser`: The currently selected user profile.
-   `financialData`: The financial data fetched from the oracle.
-   `attackFailed`: Information about a failed attack attempt.

This state is then passed down to the child components via props.

### Data Flow

The data flow is unidirectional and follows the user's progression through the steps:

1.  The user selects a profile.
2.  Financial data is fetched from the mock oracle.
3.  The data is used for the zkTLS simulation, ML scoring, and ZK proof generation.
4.  The generated proof and data are then used for the on-chain verification simulation.

### API Interaction

The frontend communicates with a mock bank oracle, which is a Node.js Express server running on `http://localhost:3001`.

-   It makes a `GET` request to `/api/financial-data` to fetch the financial data for the selected user.
-   The application includes a fallback mechanism with mock data to allow the demo to run even if the oracle is not available.

### Security Demonstrations

A key feature of the frontend is its ability to simulate and visualize security attacks:

-   **Model Tampering (Eve):** When the "Eve" user is selected, the application uses a different (tampered) ML model hash. This is then caught during the "Model Hash Match" layer of the on-chain verification.
-   **Data Tampering (Mallory):** When the "Mallory" user is selected, the application modifies the income data after it's been fetched from the bank. This is caught by the "Data Provenance" layer, which checks the data's signature.

## How to Run

1.  Make sure you have Node.js v18+ installed.
2.  Install the dependencies:
    ```bash
    npm install
    ```
3.  Start the frontend development server:
    ```bash
    npm run dev:client
    ```
4.  The application will be available at `http://localhost:5173`.
5.  For the full experience, also run the mock oracle:
    ```bash
    npm run dev:oracle
    ```
