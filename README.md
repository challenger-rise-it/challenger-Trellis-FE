# Trellis Frontend

This project is a frontend application for converting numbers to English words.

## Setup

### Prerequisites

- Node.js 22.14.0
- npm or yarn

### Installation

1. Install dependencies:

   ```sh
   npm install
   # or
   yarn install
   ```

2. Create a `.env` file in the root directory and add the following environment variable:
   ```sh
   VITE_APP_API_BASE_URL=<your-api-base-url>
   ```

### Running the Application

To start the development server, run:

```sh
npm run dev
# or
yarn dev
```

The application will be available at `http://localhost:3000`.

## Usage

1. Enter a number in the input field.
2. Click on "GET" or "POST" to get the number in English words.
3. The result will be displayed below.

## Components

- `NumberInput`: Component for inputting the number.
- `ResponseDisplay`: Component for displaying the result or error message.

## API

The application interacts with an API to convert numbers to English words. The base URL for the API should be set in the `.env` file.
