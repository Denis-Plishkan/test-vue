# NODE Version

```
v18.15.0
```

# Commands to run application

```
npm install
npm run build
```
# Linters

#### JS/TS linter: eslint + prettier
```
npm run lint:js
```

#### CSS/SCSS linter: stylelint
```
npm run lint:css
```

#### functions:


Overview of Components
1. Main Page (Country)
   Purpose: Displays a list of available countries and allows users to search for them.
   Key Features:
   1) Fetches the list of countries from an API and displays it.
   2) Implements functionality to filter countries based on user input.
   3) Supports pagination to display a limited number of countries per page.
  
2. Country Page (Public Holidays)
   Purpose: Displays public holidays for the selected country and year.
   Key Features:
   1) Fetches public holidays using the country code and selected year.
   2) Allows users to select a year to view the corresponding holidays.
   3) Implements pagination to display a limited number of holidays per page.


#### Additional Libraries and Frameworks Used:

1) Vue 3: The primary framework used for building the application, utilizing its Composition API for better state management and reactivity.

2) Vue Router: Used for navigating between different pages in the application (e.g., Main Page and Country Page).

3) Axios: A promise-based HTTP client for making API requests to fetch country data and public holidays.

4) SCSS (Sass): A CSS preprocessor used for styling the components, allowing for better organization and maintenance of styles.

5) Vuetify: A Vue UI library that provides pre-designed components such as buttons, cards, and pagination, enhancing the user interface and experience.

6) TypeScript: Used for adding type safety to the application, making the code more robust and reducing runtime errors.

