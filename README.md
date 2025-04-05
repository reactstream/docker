# ReactStream Project

This project consists of two separate services:

1. **Edit Server** - Hosts the web-based editor interface with Monaco Editor, console, and preview iframe
2. **Preview Server** - Serves component previews and renders React components

## Project Structure

```
project_root/
├── editor/                     # Główna aplikacja edytora UI
│   ├── dist/                   # Skompilowane pliki aplikacji
│   ├── public/                 # Statyczne pliki
│   ├── src/                    # Kod źródłowy React
│   │   ├── components/         # Komponenty React
│   │   │   ├── Editor.js
│   │   │   ├── Console.js
│   │   │   └── Preview.js
│   │   ├── App.js
│   │   ├── index.js
│   │   └── App.css
│   ├── server.js               # Serwer Express dla aplikacji edytora
│   ├── webpack.config.js       # Konfiguracja webpack
│   └── package.json            # Zależności aplikacji edytora
│
├── preview/                    # Serwer podglądu komponentów
│   ├── public/                 # Statyczne pliki dla podglądu
│   │   └── preview-fallback.html
│   ├── preview-server.js       # Serwer Express dla podglądu
│   ├── templates/              # Szablony dla serwera podglądu
│   │   └── example.js          # Przykładowy komponent
│   └── package.json            # Zależności serwera podglądu
│
├── codebase/                   # Usługa zarządzania kodem (Git)
│   ├── api/                    # Endpointy API
│   │   ├── routes/
│   │   │   ├── project.js      # Obsługa projektów
│   │   │   ├── session.js      # Obsługa sesji
│   │   │   └── file.js         # Operacje na plikach
│   │   └── index.js            # Główny routing API
│   ├── services/               # Usługi biznesowe
│   │   ├── git-service.js      # Obsługa operacji git
│   │   ├── session-service.js  # Zarządzanie sesjami
│   │   └── storage-service.js  # Przechowywanie danych
│   ├── models/                 # Definicje modeli
│   │   ├── project.js          # Model projektu
│   │   ├── user-session.js     # Model sesji użytkownika
│   │   └── file.js             # Model pliku
│   ├── utils/                  # Narzędzia pomocnicze
│   │   ├── git-utils.js        # Funkcje pomocnicze Git
│   │   └── session-utils.js    # Funkcje pomocnicze sesji
│   ├── server.js               # Główny plik serwera
│   ├── repositories/           # Repozytorium projektów git
│   └── package.json            # Zależności serwera kodu
│
├── docker-compose.yml          # Konfiguracja Docker Compose
└── README.md                   # Dokumentacja projektu
```

# ReactStream - Mikrousługowa aplikacja do edycji i podglądu komponentów React

ReactStream to kompleksowa platforma umożliwiająca edycję, przechowywanie i podgląd komponentów React w przeglądarce, zbudowana z trzech niezależnych mikrousług:

1. **Editor Service (Port 80)** - Główny interfejs użytkownika z edytorem kodu, konsolą i podglądem
2. **Codebase Service (Port 3020)** - Usługa przechowująca kod w repozytorium Git i zarządzająca sesjami użytkowników
3. **Preview Service (Port 3010)** - Serwer renderujący i wyświetlający komponenty React

## Architektura

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │     │                 │
│  Editor Service │ ──> │ Codebase Service│ <── │ Preview Service │
│    (Port 80)    │     │   (Port 3020)   │     │   (Port 3010)   │
│                 │     │                 │     │                 │
└────────┬────────┘     └────────┬────────┘     └────────┬────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                                 ▼
                           Repositories
                          (Stored in Git)
```

## Usługi

### Editor Service (Port 80)

- **Funkcje**: Interfejs użytkownika, edycja kodu, zarządzanie projektami, podgląd wyników
- **Technologie**: React, Monaco Editor, Socket.IO
- **Komponenty**: Editor.js, Preview.js, Console.js, ProjectSelector.js, FileExplorer.js

### Codebase Service (Port 3020)

- **Funkcje**: Zarządzanie kodem źródłowym, repozytorium Git, sesje użytkowników, historia zmian
- **Technologie**: Express.js, Simple-Git, Express-Session
- **API**: Zarządzanie projektami, plikami i sesjami

### Preview Service (Port 3010)

- **Funkcje**: Renderowanie komponentów React, wyświetlanie kodu i podglądu
- **Technologie**: Express.js, Axios
- **Endpoints**: Renderowanie komponentów, pobieranie kodu

## Zarządzanie Sesjami

System przechowuje projekty użytkownika na podstawie sesji przeglądarki:

- Każda nowa sesja otrzymuje unikalny identyfikator (UUID)
- Projekty są przechowywane w repozytorium Git i powiązane z sesją
- Sesje mogą być eksportowane, importowane i udostępniane

## Instalacja i Uruchomienie

### Wymagania

- Docker i Docker Compose
- Node.js 16+ (opcjonalnie, do uruchomienia lokalnego)
- Git

### Uruchomienie za pomocą Docker Compose

1. Sklonuj repozytorium:
   ```bash
   git clone https://github.com/username/reactstream.git
   cd reactstream
   ```

2. Uruchom usługi za pomocą Docker Compose:
   ```bash
   docker-compose up -d
   ```

3. Otwórz przeglądarkę:
   - Editor UI: http://localhost
   - Preview Service: http://localhost:3010
   - Codebase API: http://localhost:3020/api

### Uruchomienie Lokalne

1. Zainstaluj zależności dla każdej usługi:
   ```bash
   cd editor && npm install
   cd ../codebase && npm install
   cd ../preview && npm install
   ```

2. Uruchom każdą usługę w osobnym terminalu:
   ```bash
   # Terminal 1 - Editor Service
   cd editor && npm start
   
   # Terminal 2 - Codebase Service
   cd codebase && npm start
   
   # Terminal 3 - Preview Service
   cd preview && npm start
   ```

## Funkcje

- **Edycja Kodu** - Monaco Editor z podświetlaniem składni
- **Zarządzanie Projektami** - Tworzenie, wybieranie i usuwanie projektów
- **Eksplorator Plików** - Przeglądanie i zarządzanie plikami projektu
- **Historia Zmian** - Podgląd i zarządzanie historią zmian (Git)
- **Podgląd na Żywo** - Renderowanie komponentu w iframe
- **Udostępnianie** - Tworzenie linków do udostępniania projektów
- **Eksport/Import** - Zapisywanie i odtwarzanie stanu sesji



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

# Podsumowanie wygenerowanych plików projektu ReactStream

## Pliki główne
- `docker-compose.yml` - Konfiguracja Docker Compose dla trzech mikrousług
- `README.md` - Dokumentacja projektu

## Usługa Editor (port 80)
- `editor/server.js` - Serwer Express dla aplikacji edytora
- `editor/Dockerfile` - Konfiguracja Docker dla usługi edytora
- `editor/package.json` - Zależności aplikacji edytora
- `editor/webpack.config.js` - Konfiguracja webpack

### Kod źródłowy edytora
- `editor/src/index.js` - Punkt wejścia aplikacji React
- `editor/src/App.js` - Główny komponent aplikacji
- `editor/src/App.css` - Style głównego komponentu
- `editor/src/index.css` - Globalne style CSS

### Komponenty edytora
- `editor/src/components/Editor.js` - Komponent edytora Monaco
- `editor/src/components/Editor.css` - Style komponentu edytora
- `editor/src/components/Console.js` - Komponent konsoli
- `editor/src/components/Preview.js` - Komponent podglądu iframe
- `editor/src/components/Header.js` - Nagłówek aplikacji
- `editor/src/components/ProjectSelector.js` - Wybór projektów
- `editor/src/components/FileExplorer.js` - Eksplorator plików

### Usługi edytora
- `editor/src/services/api.js` - Klient API do komunikacji z Codebase Service

### Pliki statyczne
- `editor/public/index.html` - Szablon HTML aplikacji React

## Usługa Preview (port 3010)
- `preview/preview-server.js` - Serwer Express dla podglądu komponentów
- `preview/Dockerfile` - Konfiguracja Docker dla usługi podglądu
- `preview/package.json` - Zależności usługi podglądu

### Pliki szablonowe i statyczne
- `preview/templates/example.js` - Przykładowy komponent React
- `preview/public/preview-fallback.html` - Strona zastępcza dla podglądu

## Usługa Codebase (port 3020)
- `codebase/server.js` - Serwer Express dla usługi zarządzania kodem
- `codebase/Dockerfile` - Konfiguracja Docker dla usługi codebase
- `codebase/package.json` - Zależności usługi codebase

### API usługi Codebase
- `codebase/api/index.js` - Główny routing API
- `codebase/api/routes/project.js` - Zarządzanie projektami
- `codebase/api/routes/file.js` - Operacje na plikach
- `codebase/api/routes/session.js` - Zarządzanie sesjami

### Usługi Codebase
- `codebase/services/git-service.js` - Operacje Git
- `codebase/services/storage-service.js` - Przechowywanie danych
- `codebase/services/session-service.js` - Zarządzanie sesjami

## Uruchomienie projektu

Aby uruchomić projekt, wystarczy wykonać polecenie:

```bash
docker-compose up -d
```

Usługi będą dostępne pod adresami:
- Editor UI: http://localhost
- Preview Service: http://localhost:3010
- Codebase API: http://localhost:3020/api
