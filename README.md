# ReactStream Project

This project consists of two separate services:

1. **Edit Server** - Hosts the web-based editor interface with Monaco Editor, console, and preview iframe
2. **Preview Server** - Serves component previews and renders React components

## Project Structure

```
project_root/
├── edit/                      # Main editor application
│   ├── dist/                  # Compiled application files
│   ├── public/                # Static files
│   ├── src/                   # React source code
│   ├── server.js              # Express server for editor
│   ├── webpack.config.js      # Webpack configuration
│   ├── Dockerfile             # Docker configuration for editor
│   └── package.json           # Dependencies for editor
│
├── preview/                   # Component preview server
│   ├── public/                # Static files for preview
│   ├── preview-server.js      # Express server for preview
│   ├── Dockerfile             # Docker configuration for preview
│   └── package.json           # Dependencies for preview server
│
├── shared/                    # Shared files between services
│   └── example.js             # Example component
│
├── docker-compose.yml         # Docker Compose configuration
└── README.md                  # Project documentation
```

## Setup

### Prerequisites

- Node.js (v20 or higher)
- npm (v10 or higher)
- Docker and Docker Compose (optional, for containerized setup)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/reactstream/docker.git
   cd docker
   ```

2. Set up the shared directory:
   ```bash
   mkdir -p shared
   cp example.js shared/
   ```

3. Install dependencies for both services:
   ```bash
   cd edit
   npm install
   cd ../preview
   npm install
   cd ..
   ```

4. Build the editor:
   ```bash
   cd edit
   npm run build
   cd ..
   ```

## Running the Application

### Option 1: Using Docker Compose

1. Build and start the containers:
   ```bash
   docker-compose up -d
   ```

2. Access the application at http://localhost

### Option 2: Running Locally

1. Start the preview server:
   ```bash
   cd preview
   npm start
   ```

2. In a separate terminal, start the editor server:
   ```bash
   cd edit
   npm start
   ```

3. Access the application at http://localhost

## Usage

1. Edit the React component in the editor
2. Click "Update Preview" to see changes in the preview pane
3. The preview will automatically refresh with your changes

## Ports

- Editor: Port 80
- Preview: Port 3010

## License

MIT
