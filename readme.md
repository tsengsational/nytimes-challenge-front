# New York Times Challenge

This is a web application built with Vue.js that takes in data using a backend api running on a local server and serves article previews in two optional reader experiences: A flipboard-inspired book view and a standard list view.

## Getting Started

Clone the backend api, available [here](https://github.com/tsengsational/nytimes-challenge-api). And run the following command:

```
npm install
npm start
```

This should start the backend web server on `localhost:3000`

Then, in the `/nytimes-challenge-front` directory, run this following commands:

```
npm install
npm run serve
```

This should start the Vue development server on `localhost:8080`.

**NOTE:**
If your machine's local IP address is not `127.0.0.1`, you will need to change the IP address in the config file located in `./src/config.js`

## Known Issues:
- When emulating mobile devices using browser dev tools, article images will initially be sized for desktop mode. Requires a reload to resize. This is mostly an edge case since most devices requiring mobile views will have properly sized images on load. More an issue for dev environment.
- When translating from English to Martian, I assumed that numbers were not to be translated.
- When toggling back and forth between List and Book views, the app scrolls to the top of the page in order for the Book pages to be visible. It also resets the pages and unflips them when toggling back to Book view. This might be an issue for readers who might feel that they lose their position.
