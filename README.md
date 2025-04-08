# Venue Management App (BODY GYM)

This project is a web-based application designed for managing a venue, specifically tailored for a gym environment like "BODY GYM". It provides separate interfaces for administrators and clients.

## Features

### 1. Admin Panel (`index.html`)

*   **✨ Client Management:**
    *   Add new clients (CIN, phone, name, optional first payment).
    *   View a list of all clients with status (Overdue, Urgent, Up-to-date).
    *   Search clients by CIN, phone number, or name.
    *   Edit existing client information.
    *   Delete clients (with confirmation).
*   **💰 Payment Tracking:**
    *   Record monthly payments against the global tariff.
    *   Record payments for special offers (fixed duration or one-day).
    *   View detailed payment history for each client.
    *   Delete individual payments or entire offer payment sequences.
*   **💲 Global Tariff:**
    *   Set and update the standard monthly fee for the venue.
*   **🏷️ Offer Management:**
    *   Create custom payment offers (e.g., "3 months offer", "1 day pass").
    *   Define offer duration and total price (calculates monthly equivalent).
    *   View, edit, and delete existing offers.
*   **🗓️ Schedule Management:**
    *   Define daily opening and closing hours for the entire week.
    *   Mark specific days as closed or exceptionally closed.
    *   View the current week's schedule in a table format.
*   **📢 Communication:**
    *   **Public Messages:** Post announcements visible to all clients in their portal (with an expiry date).
    *   **Private Messages:** Engage in private, real-time chat conversations with individual clients.
*   **📊 Data Export/Import:**
    *   Export the current client list (including next payment date) to an Excel file (`.xlsx`).
    *   Export the weekly opening hours schedule to a Word document (`.docx`).
    *   Export the list of created offers to a Word document (`.docx`).
    *   *(Note: PDF export for offers seems partially implemented but might need review)*.
    *   Import clients from a formatted Excel file.

### 2. Client Portal (`espace client/client2.html`)

*   **🔒 Secure Login:** Clients log in using their registered Phone Number and CIN.
*   **👤 Profile View:**
    *   Displays client's name.
    *   Shows profile picture (with upload/change functionality).
    *   Displays current gym status (Open/Closed).
    *   Shows the date of their next payment.
    *   Displays the remaining days on their current subscription/offer.
    *   Shows the applicable monthly tariff.
*   **🗓️ Schedule View:**
    *   Displays the full weekly opening hours calendar.
*   **📢 Messages:**
    *   View active public announcements from the administration.
    *   View and send private messages to/from the administration in a chat interface (with unread indicators).
*   **🔗 Quick Links:**
    *   Direct links to the venue's WhatsApp, Instagram, and Google Maps location.
*   **(Self-Service Offers - Needs Review):** Functions exist to potentially allow clients to select pre-defined offers (e.g., Day Pass), but the UI integration might need completion.

### 3. Overdue Clients Page (`public/clients.html`)

*   A dedicated page accessible from the Admin panel.
*   Displays a filtered list of clients whose payments are overdue or due within the next 5 days.
*   Shows remaining days or days overdue.
*   Includes a counter for the number of clients needing follow-up today.

## Technology Stack

*   **Frontend:** HTML, CSS, JavaScript
*   **Backend/Database:** Firebase Realtime Database
*   **Firebase Services:**
    *   Firebase Realtime Database (Primary data storage)
    *   *(Firebase Storage likely needed for Profile Picture uploads)*
    *   *(Firebase Authentication could be integrated for more robust login)*
*   **Libraries:**
    *   Font Awesome (Icons)
    *   SheetJS (xlsx) (Excel parsing/generation)
    *   SweetAlert2 (Enhanced alert popups)
    *   jsPDF (PDF generation - used in offer export)
    *   docx (Word document generation)
    *   FileSaver.js (Saving generated files)

## Setup

1.  **Clone Repository:**
    ```bash
    git clone <your-repository-url>
    cd venue-management-app
    ```
2.  **Firebase Project Setup:**
    *   Create a new project on the [Firebase Console](https://console.firebase.google.com/).
    *   Add a Web App to your project.
    *   Copy the Firebase configuration object provided.
    *   Enable **Realtime Database**. You might start in Test Mode for initial development, but **remember to set up proper Security Rules** before production:
        ```json
        // Example (Restrictive - adjust as needed!)
        {
          "rules": {
            // Allow admin full access (you'll need a way to identify admin)
            // Allow authenticated clients read access to their data, messages, offers, schedule
            // Allow authenticated clients write access only to their messages node
            ".read": "auth != null",
            ".write": "auth != null" // VERY permissive, tighten this!
          }
        }
        ```
    *   Enable **Firebase Storage** if you intend to use the profile picture upload feature. Configure its security rules appropriately.

3.  **Add Firebase Configuration:**
    *   Open `index.html` and `espace client/client2.html`.
    *   Find the `firebaseConfig` JavaScript object variable near the top of the `<script type="module">` block.
    *   Replace the placeholder values with the actual configuration values from your Firebase project.
    *   **IMPORTANT:** Do NOT commit your actual Firebase API keys and config details to a public GitHub repository. Use environment variables or a gitignored configuration file for production deployments.

4.  **Run the Application:**
    *   Simply open the `index.html` (for the admin panel) or `espace client/client2.html` (for the client portal) files in your web browser.
    *   For features relying on JavaScript modules (`type="module"`), you might need to serve the files using a local web server (e.g., using the Live Server extension in VS Code or `python -m http.server` in your terminal) rather than opening the file directly via `file:///`.

## Usage

*   **Admin:** Access `index.html` to manage clients, schedules, offers, and messages.
*   **Client:** Access `espace client/client2.html` and log in using the phone number and CIN registered by the admin.
