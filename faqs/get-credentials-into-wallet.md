# How can I get credentials into my Meeco Wallet from SVX Portal?


To get credentials into your Meeco Wallet, make sure that the connection between Organisation and Wallet Holder is established. If no, please do the following steps as an Organisation Administrator:

1. Login to the SVX Portal
2. Navigate to the organisation dashboard
3. Go to Enterprise Vault > Connections
4. Press the button “Generate invitation code”
5. Provide an identifier that enables you to know who you will send the code to (e.g. name of the person)
6. Press “Generate invitation code”
7. It will show a code, that is important for the next step

The next steps are expected from the Meeco Wallet Holder and their application:

1. Navigate to Settings > Organisation configuration
2. Press the button “Enter invitation code”
3. Enter the code that has been generated earlier in the first field
4. Specify any name in the second field (usually we use the name of the organisation/issuer)

***What we have done now, is created a connection between the organisation and the end-user. This is important if we are using the SVX Portal to issue credentials.***

Now you can use the SVX Portal to issue the credential:

1. Go to Credentials > Issue / Revoke Credentials
2. Press the button “Issue credential”
3. Select the “Select connection” option and choose the identifier that you set earlier
4. Proceed with issuing the credential

***Now, when you open the Wallet switch tabs or pull down on the credential screen, it will detect that a credential is shared with you via the connection that we created earlier.***
