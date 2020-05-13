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

4. use below commands to use a revision of the spreadsheet
  
    A. npm run build:langs
    ```
    inside of packages.json
    "build:langs": "rm -fr build/locales && npm run build:babel && translation build"
    ```
    The latest version of the spread sheet will be used
    json files for languages will be generated under the build directory (build/locales)  
    
    B. npm run build:langsRev
    ```
    inside of packages.json
    "build:langsRev": "rm -fr build/locales && npm run build:babel && translation build -r [revisionID]"
    ```
    The version with [revisionID] of the spread sheet will be used
    If [revisionID] is unknown, please try C first.
    json files for languages will be generated under the build directory (build/locales)  
    
    C. npm run download:langs
    ```
    inside of packages.json
    "download:langsRev": "translation downloadOnly"
    ```
    it will show **all revision list** and let user choose [revisionID]  
    json files for languages will be generated under the revision directory (build/locales/revisions/[revisionID])  

    D. npm run download:langsRev
    ```
    inside of packages.json
    "download:langsRev": "translation downloadOnly -r [revisionID]"
    ```
    The version with [revisionID] of the spread sheet will be used
    json files for languages will be generated under the revision directory (build/locales/revisions/[revisionID])  
