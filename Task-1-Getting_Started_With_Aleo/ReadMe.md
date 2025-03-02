# Getting Started with ALEO:

## Create new project:
- Command:
    ```sh
    leo new gcsita2626_leo_bidder_auction
    ```

- Create 3 accounts. Owner, Bidder1, Bidder2.
- Command:
    ```sh
    snarkos account new
    ```
- So, run above command 3 times in terminal:
    <details><summary> Detailed Output </summary><blockquote>

    ~~~
    $ snarkos account new

    Private Key  <redacted>
        View Key  <redacted>
        Address  aleo19ltc688wvmtez9t767r6jxu569e09vhaycx9v6hh689dd3jvs5qqrd7slr

    $ snarkos account new

    Private Key  <redacted>
        View Key  <redacted>
        Address  aleo1k42w6vqcvyehqgsr4j4j2qxwjyfrwdrhr7erwlvqdj2wc8vhxsgqytsv72

    $ snarkos account new

    Private Key  <redacted>
        View Key  <redacted>
        Address  aleo120necdjm8dmuws027h6hunzp57j9flde00cnlf5zzps558rnugxs3uzxyv
    ~~~
    
    </blockquote></details>

- In above output, suppose 1 a/c with Address `aleo19ltc688wvmtez9t767r6jxu569e09vhaycx9v6hh689dd3jvs5qqrd7slr` is owner and remaining 2nd and 3rd a/cs are bidders. 
- Make sure `ENDPOINT` in `.env` file is `https://api.explorer.provable.com/v1`.
     
## Deploy To Testnet:
- We need ALEO token in each accounts. So load up before moving any further.
- Now, the value of `PRIVATE_KEY` in `.env` must be replaced with the `PRIVATE_KEY` of the Owner.
- Command:
    ```sh
    leo deploy --network testnet -y
    ```
    <details><summary> Detailed Output </summary><blockquote>

    ~~~
       Leo ✅ Compiled 'gcsita2626_leo_bidder_auction.aleo' into Aleo instructions
    📦 Creating deployment transaction for 'gcsita2626_leo_bidder_auction.aleo'...

    Base deployment cost for 'gcsita2626_leo_bidder_auction.aleo' is 14.504325 credits.

    +------------------------------------+----------------+
    | gcsita2626_leo_bidder_auction.aleo | Cost (credits) |
    +------------------------------------+----------------+
    | Transaction Storage                | 3.641000       |
    +------------------------------------+----------------+
    | Program Synthesis                  | 9.863325       |
    +------------------------------------+----------------+
    | Namespace                          | 1.000000       |
    +------------------------------------+----------------+
    | Priority Fee                       | 0.000000       |
    +------------------------------------+----------------+
    | Total                              | 14.504325      |
    +------------------------------------+----------------+

    Your current public balance is 48.848612 credits.

    ✅ Created deployment transaction for 'gcsita2626_leo_bidder_auction.aleo'

    Broadcasting transaction to https://api.explorer.provable.com/v1/testnet/transaction/broadcast...

    ⌛ Deployment at1u837hzukcyj4waqa2kr972z0l53cc380sc6jf0gms2q0kw9awsqqd7l8qz ('gcsita2626_leo_bidder_auction.aleo') has been broadcast to https://api.explorer.provable.com/v1/testnet/transaction/broadcast.
    ~~~

    </blockquote></details>


- On-Chain Outputs: 
    - Deployment ID: `at1u837hzukcyj4waqa2kr972z0l53cc380sc6jf0gms2q0kw9awsqqd7l8qz`
    - Aleo Program: [https://testnet.aleo.info/program/gcsita2626_leo_bidder_auction.aleo](https://testnet.aleo.info/program/gcsita2626_leo_bidder_auction.aleo)
    - Deploy Txn: [https://testnet.aleo.info/transaction/at1u837hzukcyj4waqa2kr972z0l53cc380sc6jf0gms2q0kw9awsqqd7l8qz](https://testnet.aleo.info/transaction/at1u837hzukcyj4waqa2kr972z0l53cc380sc6jf0gms2q0kw9awsqqd7l8qz) 


## Execution Details:
- 1st Bidder Execution:
    - Transition ID: `at18z8v463s98vx7hgdwp59a3s9kd3s6axpafywv5vu72cfav5k2gpsdr8y43`
    - Output Record: `record1qyqspudz7q8jgquwjpxh665uam7zrqwm6er3dyavwju0q5qvajhxu4g3qvrxy6tyv3jhyscqqgpqqrtrcemad7rngtgchghl90hy0hxvqwhqksrsds2gf6c7k25p8ygx5v77g25gak53jgtxcdnmzs86asxz0fud20fnyvql7w5w5x56tc8qvctdda6kuaprqqpqzqpl9m0364vcmlnchwsj00z6w5ugcju2dxpfq6t252j2z506paypzyykju6lwa5kumn9wg3sqqspqp5r5dnwp96hacz38y9q9lv4ht9hnu853e4a7p7025sf5fedgkjp9fz5tn79awwcecsv22ak6cxn0nhse86s6w0ne93rljc82pysxkszp5qvre`

- 2nd Bidder Execution:
    - Transition ID: `at13s577hlgudp3av6wnjekutnn2de66qsq4a7fy0f07a3gqvh3qsyqq3f8cv`
    - Output Record: `record1qyqspj5xtqjr0wxdw26p2ex6h3369cjnv8mnyvvp68shdg5epwguxtcgqvrxy6tyv3jhyscqqgpqqcxwmhk375e37r7xel4cf6ms8yy75yse5hn5yn067kh5fa0k43cdfzhthf28255x9snjy6cr3z2p8d6h2lxh5dl7ewgrm695v0ygwv9svctdda6kuaprqqpqzqxygdx9kzceqt579dalz2n6krtfvg2m6luyngqssvr403vujvc2qcykju6lwa5kumn9wg3sqqspqr70hht6px6j696gjm8xmfrgyuv28q3jxlewrvzwuammkjg3e28ss5et0fuy4vj052lv8qm5errnrfqntqxxec9v627cau4yytae8zqd8ff3k2`

- Decrypt Record using:
    ```sh
    snarkos developer decrypt -v <VIEW_KEY> -c <RECORD_CIPHERTEXT>
    ```

- Resolve Bid:
    - Transition ID: `at1u0htjnnvwz0g5wvks0ah9m688tnxyhp2gfr9tsc09fu4hggte5pquyvqxp`
    - Output Record: `record1qyqsqsqjg2yqzvnufrss269pkyug80y340hd5x2dpqjx9g94vefargq8qvrxy6tyv3jhyscqqgpqq8fhuyqw0rl86vd3q4m2t4y38aaesa3p6rrg0xrxkfmcseg5gsgq7kprh8mgsjkajjltu8x6k6leekctqqszpwamygdp0mzf6ax8dc9svctdda6kuaprqqpqzqqk6l839y53w72q89sj0cnl4ft43nne7q2qfxpt7wpjp8898f5aqyykju6lwa5kumn9wg3sqqspqq4h20zcwglaeemt3r7wmls8ktl0rg2aev77rmpfumqyap7j3nzsf5wapj6nqru3xu0r6j5m0z7m08z87fhjrp76ypxy2deyr5kcw3gwevw9dt`

- Finish Bid:
    - Transition ID: `at1aevqyxmf9avx8rq38xcgk3uh95m5ldamtunxgrs6skcljgm00g8sddvcel`
    - Output Record: `record1qyqspnp8s9vpk0cc5e6jv2ufy54rzqegmmf28fj7lc5svqsw2xwfjecvqvrxy6tyv3jhyscqqgpqpc86j7lys8cwxxeg5p8urtravrc20cah2unl0ascjujln990wes95enw2vsam7nyvr3cxhwsd4uw8vwsj7kzqvq9y85nyqukzmata5gsvctdda6kuaprqqpqzqx3xvu4cudhjnkyqh3dx99fteluh7uhgu5t62rjk69haa63sch5qqykju6lwa5kumn9wg3sqqspqp46vp4agl99ef8e672xxw98rq3s9l736ut2kw5wrlgdm0ynylns2pznatzajla5hq30f8zrlu9ufsfxcyvs3pd6w4hm25jsqc5mmecqrrdwue`

- Claim Bid:
    - Transition ID: `at1jp0hrkh7vejq9xvcqeqf5rqyd2lv7vlle9m4ngmjc80rn8hwavgs28pq9r`
    - Output Record: `record1qyqsq5ff3ylj366c2ra3tp3fe4na6jndq8a824l26nzh636f3hqwuygsqyrxzmt0w4h8ggcqqgqspntszh7ftvf879qhdn57z2xelcvx2hdzuake267f57gpv67t9kcsk2hzzdjf80r5lz5e68a9vfcvqsh7nttakdhnvvq96n9zegu50y9suruy0a`

# Signature:
## Sign with `true`:
- Command:
    ```sh
    leo account sign --private-key <redacted> --message true
    ```

- Output:
    ```sh
    sign1qkrrfsnqruewgqqxw7n95vlgs0kjpppgpnvlwsww3ax6yyuw65pyy96f828chdgv4xntqnlj5pxxd5dhl4dyjvxvpvpyss9uphjpzqtu23rr6mhkgkrusjsmmc7taewueergms2yf25r2fxqy9fphr4aqrj4uvr5pmmj9hcjx0g5kvp3mxavfe7fq5ppdgjnkpyg65u0acrsc8da4j4
    ```

## Sign with `Transaction ID`:
- For me, program deployed Transaction ID is: `at1u837hzukcyj4waqa2kr972z0l53cc380sc6jf0gms2q0kw9awsqqd7l8qz`. Command:
    ```sh
    leo account sign -d --private-key <redacted> --message "at1u837hzukcyj4waqa2kr972z0l53cc380sc6jf0gms2q0kw9awsqqd7l8qz" --raw
    ```
- Output:
    ```sh
    sign1hm3khepp8g0lst3ct0jfedwwx2vhznnk5kt7e3yj7vxwvc5yqvqav2mfk9qj7k75cq55waghhjfn25duw9ap6pwhwg8w3r56x9v5cqru23rr6mhkgkrusjsmmc7taewueergms2yf25r2fxqy9fphr4aqrj4uvr5pmmj9hcjx0g5kvp3mxavfe7fq5ppdgjnkpyg65u0acrscdmnkh2
    ```