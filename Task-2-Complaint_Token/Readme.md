# time Complaint Token:

## Create leo Project:
- Command:
    ```sh
    leo new gcsita26_kyc_complaint_token
    ```

## Install dependencies:
- Command:
    ```sh
    cd gcsita26_kyc_complaint_token
    leo add token_registry
    leo add credits
    ```

## Compiled Program:
- Command:
    ```sh
    leo build
    ```

## Deploy to testnet:
- Value of Endpoint must be changed with: `https://api.explorer.provable.com/v1` in `.env` file.
- Now, the value of `PRIVATE_KEY` in `.env` must be replaced.
- We will require some token to deploy our program into testnet.

- Command:
    ```sh
    leo deploy --network testnet
    ```
    
    <details><summary> Detailed Output </summary><blockquote>

    ~~~sh
       Leo ✅ Compiled 'gcsita26_kyc_complaint_token.aleo' into Aleo instructions
    📦 Creating deployment transaction for 'gcsita26_kyc_complaint_token.aleo'...


    Base deployment cost for 'gcsita26_kyc_complaint_token.aleo' is 10.203575 credits.

    +-----------------------------------+----------------+
    | gcsita26_kyc_complaint_token.aleo | Cost (credits) |
    +-----------------------------------+----------------+
    | Transaction Storage               | 4.423000       |
    +-----------------------------------+----------------+
    | Program Synthesis                 | 4.780575       |
    +-----------------------------------+----------------+
    | Namespace                         | 1.000000       |
    +-----------------------------------+----------------+
    | Priority Fee                      | 0.000000       |
    +-----------------------------------+----------------+
    | Total                             | 10.203575      |
    +-----------------------------------+----------------+

    Your current public balance is 18.775306 credits.

    ? Do you want to submit deployment of program `gcsita26_kyc_complaint_token.aleo.aleo` to network testnet via endpoint https://api.explorer.provable.com/v1 using 
    ✔ Do you want to submit deployment of program `gcsita26_kyc_complaint_token.aleo.aleo` to network testnet via endpoint https://api.explorer.provable.com/v1 using address aleo19ltc688wvmtez9t767r6jxu569e09vhaycx9v6hh689dd3jvs5qqrd7slr? · yes
    ✅ Created deployment transaction for 'gcsita26_kyc_complaint_token.aleo'

    Broadcasting transaction to https://api.explorer.provable.com/v1/testnet/transaction/broadcast...

    ⌛ Deployment at1mhqxcg0dax89q5rea6c7pkt2vyn99y4vs3kv2wpjrnhdtv0dcg9skdv3uk ('gcsita26_kyc_complaint_token.aleo') has been broadcast to https://api.explorer.provable.com/v1/testnet/transaction/broadcast.

    ~~~

    </blockquote></details>

- On-Chain Outputs: 
    - Transaction ID: `at1mrgl3z89mptx3jgzp6xjvsvuj9d9d9c89mrwu5rrdh6jk8fq55zszv4ykq`
    - Aleo Program: [https://testnet.aleo.info/program/gcsita26_kyc_complaint_token.aleo](https://testnet.aleo.info/program/gcsita26_kyc_complaint_token.aleo)
    - Deploy Txn: [https://testnet.aleo.info/transaction/at1mrgl3z89mptx3jgzp6xjvsvuj9d9d9c89mrwu5rrdh6jk8fq55zszv4ykq](https://testnet.aleo.info/transaction/at1mrgl3z89mptx3jgzp6xjvsvuj9d9d9c89mrwu5rrdh6jk8fq55zszv4ykq) 


# Signature:

## Sign with `Transaction ID`:
- For me, program deployed Transaction ID is: `at1mrgl3z89mptx3jgzp6xjvsvuj9d9d9c89mrwu5rrdh6jk8fq55zszv4ykq`. Command:
    ```sh
    leo account sign -d --private-key <redacted> --message "at1mrgl3z89mptx3jgzp6xjvsvuj9d9d9c89mrwu5rrdh6jk8fq55zszv4ykq" --raw
    ```
- Output:
    ```sh
    sign1g5s57x5cjaydmt6edcwf9nc00ueua0mke2ujsyk8jazm2drkpgpnea479mn8pwg3cks8lq05zk2z5x4ndu0230ysf0htv34tm505sqmu23rr6mhkgkrusjsmmc7taewueergms2yf25r2fxqy9fphr4aqrj4uvr5pmmj9hcjx0g5kvp3mxavfe7fq5ppdgjnkpyg65u0acrscsetlsk
    ```