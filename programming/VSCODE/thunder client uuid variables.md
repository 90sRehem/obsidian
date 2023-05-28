#vscode #ide  #thunder-client

link to docs: [link](https://github.com/rangav/thunder-client-support)

## Path Variables

Path variables are supported using the format `{variable}` in the url field:

-   **Example 1**: `https://www.thunderclient.com/details/customer/{customerId}`
-   **Example 2**: `https://www.thunderclient.com/details/{customerId}{name}/`

## System Variables

System variables are useful to generate random/dynamic data for use in request query params or body. The format is `{{#variableName}}`

-   {{#guid}} — Generates random uuid.
-   {{#name}} - Generates random name.
-   {{#string}} — Generates random string.
-   {{#number}} — Generates random number between 1 to 1000000.
    -   **Custom Range**: use `{{#number, min, max}}`
    -   Example: `{{#number, 100, 999}}`
-   {{#email}} — Generates random email.
-   {{#bool}} — Generates true or false.
-   {{#enum, val1, val2, val3,...}} — Generates one of the enum values provided (comma separated).
    -   example: `{{#enum, red, green, blue}}`
    -   example: `{{#enum, 1, 2, 3}}`
-   {{#date}} — Generates unix date timestamp in milliseconds.
    -   **Custom date format**: use `{{#date, 'YYYY-MM-DD hh:mm:ss:fff'}}`, the format should be in `single` quotes.
    -   Unix timestamp: use `{{#date, 'X'}}`, this will output unix timestamp in seconds.
    -   **Manipulate date** using format : `{year:2, mon:-3, day:-2, hour:3, min:5, sec:7}`
        -   Example 1: `{{#date, {year: -1, day: 3, mon: 5}}}`
        -   Example 2: `{{#date,'YYYY-MM-DD', {year: -1}}}`
-   {{#dateISO}} — Generates date ISO format.
    -   **Manipulate date** using format : `{year:2, mon:-3, day:-2, hour:3, min:5, sec:7}`
        -   Example 1: `{{#dateISO, {year: 1}}}`
        -   Example 2: `{{#dateISO, { year : -1, day: 3 } }}`