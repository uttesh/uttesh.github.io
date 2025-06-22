# AR Location-Based Web Application

This project is a simple augmented reality (AR) web application that leverages the user's geographical location to display virtual 3D items placed randomly in their real-world environment. It's built using HTML, JavaScript, [A-Frame](https://aframe.io/), and [AR.js](https://ar-js-org.github.io/AR.js-Docs/).

## ✨ Features

- **Location-Based AR:** Utilizes your device's GPS to anchor virtual objects to real-world coordinates.

- **Dynamic Item Placement:** Places a configurable number of random 3D items around your current location.

- **Mobile-Optimized:** Designed to run directly in a mobile web browser, requiring no app installation.

- **Camera Integration:** Overlays 3D content directly onto your device's camera feed.

- **Basic Interaction:** Users can tap on displayed items to trigger a simple message.

- **User Feedback:** Includes a loading overlay and a custom message box for permissions and errors.

## 🚀 Getting Started

To get this AR web application up and running, follow these steps:

### 1. Clone or Download the Project

First, get the project files. You can clone this repository (if it were a Git repo) or simply create the `index.html` file and an `assets` folder as described below.

```
# Example if using Git (replace with your actual repo)
git clone <repository-url>
cd <project-folder>
```

### 2. Project Structure

Ensure your project has the following structure:

```
my-ar-app/
├── index.html
└── assets/
    ├── item1.glb
    ├── item2.glb
    ├── item3.glb
    ├── item4.glb
    └── ... (more .glb models)
```

- **`index.html`**: Contains all the HTML structure, A-Frame scene, AR.js configuration, and JavaScript logic.

- **`assets/`**: This directory will hold your 3D models.

### 3. Obtain 3D Models (`.glb` files)

The application is set up to display `.glb` (GLB) 3D models. You need to download or create these models and place them in your `assets/` folder.

**Where to find free `.glb` models:**

- **Sketchfab:** A vast repository of 3D models. Use the "Downloadable" and "glTF" or "GLB" filters. Always check licenses for attribution requirements.

- **Free3D:** Offers a selection of free 3D models, many available in GLB format.

- **CGTrader & TurboSquid:** While known for paid models, both have free sections you can filter.

- **Poly Pizza:** Provides low-poly, downloadable assets.

**How to create your own `.glb` models:**

- **Blender:** A free and open-source 3D creation suite. You can model, texture, and export directly to `.glb` from Blender. There are many tutorials available online for beginners.

**Once you have your models:**

1.  Place them inside the `assets/` folder (e.g., `assets/my_cool_item.glb`).

2.  Update the `itemsToPlace` array in `index.html` to point to your model paths:

    ```javascript
    const itemsToPlace = [
      {
        name: "My First Model",
        model: "./assets/my_cool_item.glb",
        scale: "1 1 1"
      }
      // ... add more items
    ];
    ```

    **Note:** In the provided code, placeholder image URLs were used for demonstration. For actual 3D models, you would use `gltf-model` attribute instead of `material` and `geometry`.

### 4. Serve the Application over HTTPS

**This is a critical step.** Modern web browsers require your application to be served over **HTTPS** to access the device's camera and geolocation features.

**Recommended Methods:**

- **GitHub Pages:** A simple and free way to host static websites over HTTPS.

  1.  Create a new GitHub repository.

  2.  Upload your `my-ar-app` folder (containing `index.html` and `assets/`).

  3.  Go to your repository settings on GitHub, navigate to "Pages," and select the branch where your code resides (e.g., `main` or `master`) as the source for GitHub Pages.

  4.  GitHub will provide you with an HTTPS URL (e.g., `https://yourusername.github.io/your-repo-name/`).

- **Local Server with Node.js (`http-server`) and HTTPS (for testing):**

  1.  **Install Node.js:** If you don't have Node.js installed, download it from [nodejs.org](https://nodejs.org/).
  2.  **Install `http-server`:** This is a simple, zero-configuration command-line http server.
      ```bash
      npm install -g http-server
      ```
  3.  **Generate Self-Signed Certificates (for HTTPS):**
      - You'll need `openssl` installed (usually pre-installed on Linux/macOS, available for Windows via Git Bash or WSL).
      - Run these commands in your `my-ar-app` directory to create `cert.pem` and `key.pem`:
        ```bash
        openssl genrsa -out key.pem 2048
        openssl req -new -key key.pem -out csr.pem
        openssl x509 -req -days 365 -in csr.pem -signkey key.pem -out cert.pem
        ```
        (You can press Enter for most prompts, or fill in details as you wish. Common Name/Hostname should be `localhost` for local testing.)
  4.  **Start `http-server` with HTTPS:**
      ```bash
      cd my-ar-app
      http-server -S -C cert.pem -K key.pem -p 8000
      ```
      (The `-S` enables SSL, `-C` specifies the certificate file, `-K` specifies the key file, and `-p` sets the port.)
  5.  Open `https://localhost:8000` in your browser. You will likely see a privacy warning because it's a self-signed certificate; you'll need to proceed past this warning (e.g., click "Advanced" then "Proceed to localhost").

- **Local Server with `ngrok` (for exposing local server to public HTTPS):**

  1.  Install `ngrok` (download from [ngrok.com](https://ngrok.com/)).
  2.  Start a simple local web server first (e.g., using `http-server` from the previous step on port 8000, but without `-S -C -K` since `ngrok` handles HTTPS):
      ```bash
      cd my-ar-app
      http-server -p 8000
      ```
  3.  In a new terminal, expose your local server to the internet via `ngrok`:
      ```bash
      ngrok http 8000
      ```
  4.  `ngrok` will provide you with a public HTTPS URL (e.g., `https://xxxxxx.ngrok-free.app`). Use this URL on your mobile device.

- **Other Hosting Services:** Netlify, Vercel, Firebase Hosting, etc., also provide easy HTTPS deployment for static sites.

### 5. Run on Your Mobile Device

1.  Open the HTTPS URL of your deployed application in your mobile browser (e.g., Chrome, Safari).

2.  The browser will prompt you to **allow camera access** and **allow location access**. You must grant both permissions for the AR experience to work.

3.  Once loaded, look around with your device. You should see the real world through your camera, with the randomly placed 3D items appearing in their designated locations.

4.  Tap on an item to see a brief message.

## ⚙️ Configuration

You can customize the application by modifying the `index.html` file:

- **`itemsToPlace` array:**

  - Add or remove items.

  - Change `name`, `model` path, and `scale` for each item.

  - For actual 3D models, make sure `model` points to your `.glb` file and use the `gltf-model` attribute on `a-entity` instead of `material` and `geometry`.

- **Number of items:** Change the `for` loop in `placeRandomItems` (e.g., `for (let i = 0; i < 5; i++)`) to place more or fewer items.

- **Item spread:** Adjust the `0.0005` value in `offsetLat` and `offsetLon` to control how far apart the items are scattered around the user's location. A larger value creates a wider spread.

- **Geolocation options:** Modify `enableHighAccuracy`, `timeout`, and `maximumAge` in `navigator.geolocation.getCurrentPosition()` options for different location precision and caching behaviors.

## ⚠️ Important Considerations

- **HTTPS is mandatory:** Geolocation and camera access will not work without it.

- **GPS Accuracy:** The precision of item placement depends directly on your device's GPS accuracy, which can vary based on environment (indoors vs. outdoors, urban vs. open areas).

- **Performance:** While AR.js is lightweight, placing a very large number of complex 3D models can impact performance on less powerful mobile devices. Optimize your `.glb` models for web use (lower polygon counts, optimized textures).

- **Battery Usage:** AR applications can consume significant battery due to continuous camera, GPS, and graphics processing.

## 🤝 Contributing

Feel free to fork this project, submit pull requests, or open issues if you have suggestions or encounter problems.

Enjoy exploring your surroundings with augmented reality
