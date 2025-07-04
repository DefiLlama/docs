How to update metadata

Step 1. Fork defillama-server

Step 2. Check data.ts or data2.ts to find your project listing

    - find the part you would like to update (i.e. name, token address or symbol, description, twitter handle, etc..)
    - commit your changes on your branch and submit a PR

Step 3. If you need to add an audit link

    - add "audit_links" to the listing and add your audit link as a "string" within an array
        - compare with others that already have an audit link added
    - change the audits portion of the listing to the correct number
        -Audits: Please follow this legend
            0 -> No audits
            1 -> Part of this protocol may be unaudited
            2 -> Yes
            3 -> This protocol is a fork of an existing audited protocol

Step 4. If you need to update the logo

    - make sure that you also have defillama-app repo
    - check the name for the logo on the listing (it should be the same as the protocol name) 
    - upload the new logo with the same name to defillama-server/defi/icons
    - commit your changes and submit the pull request
    - also upload a smaller logo 24x24 pixel & .jpg format to defillama-app/public/icons
    - commit your changes and submit the pull request

