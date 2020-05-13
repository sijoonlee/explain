# Explain add-revision-downloader  
  
## how to setup Web-server in front-end  
1. Enable google drive api from [Google's console page](https://console.developers.google.com/apis/dashboard)
  
2. Delete token.json under directory '.credentials' to re-initiate authentification  
    - this is because google drive read-only access is requried
    - current token doesn't provide the access
  
3. update [lang.conf.js](https://github.com/sijoonlee/explain/blob/master/lang.conf.js) like below
```
  // This is an untranspiled Node file, thus violates a bunch of our suggested ESLint rules
/* eslint-disable */

module.exports = {
    build_path: 'build/locales/',
    revisions_path: 'build/locales/revisions/',
    credentials_path: '.credentials/',
    messages_glob: '../{!(omp-app),mortgages/!(mtg-app-domain),insurance/*,credit-cards/*,investing/*}/dist/messages/**/*.json',
    spreadsheet_id: '---copy-and-paste-from-original-lang.conf.js---',
    xlsx_file_name: 'lang.xlsx',
    langs: [
        {
            lang_code: 'en',
            sheet_id: '0',
            sheet_name: 'en (default)',
            is_default: true
        },
        {
            lang_code: 'fr',
            sheet_id: '1036589945',
            sheet_name: 'fr',
        }
    ],
    htmlSanitizer:
    {
        allowedTags: [ 
            'h3', 'h4', 'h5', 'h6', 'blockquote', 'p', 'a', 'ul', 'ol',
            'nl', 'li', 'b', 'i', 'strong', 'em', 'strike', 'code', 'hr', 'br', 'div',
            'table', 'thead', 'caption', 'tbody', 'tr', 'th', 'td', 'pre', 
            'small', 'span', 'u', 'output'
        ]
    }
};
```
4. use below lines in package.json to use a revision of the spreadsheet
    ```
    "build:langs": "rm -fr build/locales && npm run build:babel && translation build -r [revisionID]",
    ```
    - option **-r** **RevisionID**  
    if you want to download a specific revision with known revision id, use this option.  
    if ommitted, it will use the latest version of the spreadsheet
    json files for languages will be generated under the build directory (build/locales)

    ```
    "download:langsRev": "translation downloadOnly -r [revisionID]"
    ```
        - option **-r** **RevisionID**  
    if you want to download a specific revision with known revision id, use this option.  
    if ommitted, it will show simple dialog for user to choose [revisionID]
    json files for languages will be generated under the revision directory (build/locales/revisions/[revisionID])
