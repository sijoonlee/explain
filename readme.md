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
4. add two lines into package.json under web-server  
    ```
    "build:langsRev": "translation downloadAndBuild",
    "download:langsRev": "translation downloadOnly -r 31"
    ```
    - option **-r** **RevisionID**  
    if you want to download a specific revision with known revision id, use this option.  
    if you don't use option, dialog will show up for you to choose Revision ID

5. npm run build:langsRev  
    this commands will   
    1) download a xlsx file of a speicic Revision under build/locales/revisions/[revisionID]
    2) parse the xlsx file
    3) save json files under **build/locales**
   *this will **OVERWRITE** json files for web-server build*

6. npm run download:langsRev  
    this commands will   
    1) download a xlsx file of a speicic Revision under build/locales/revisions/[revisionID]
    2) parse the xlsx file
    3) save json files under **build/locales/revisions/[revisionID]**
   *this will NOT overwrite json files for web-server build*

## Demo
<p align ="center"> 
  <img src = "https://github.com/sijoonlee/explain/blob/master/download_ex.gif" width = "700"/> 
</p>

<p align ="center"> 
  <img src = "https://github.com/sijoonlee/explain/blob/master/build_ex.gif" width = "700"/> 
</p>


