# Building Zefi App

## Create new leo Project:
```sh
leo new gcsita_token_vesting_linear
```

## Setup project:
```sh
cd gcsita_token_vesting_linear
leo add token_registry
leo add credits
```

## Modified `main.leo`:
- Paste below contents inside `main.leo` but change value of address with your public address.:

    <details><summary> Detailed Output </summary><blockquote>

    ~~~sh
    // The 'gcsita_token_vesting_linear' program.
    import token_registry.aleo;
    import credits.aleo;

    program gcsita_token_vesting_linear.aleo {
        const ADMIN: address = aleo19ltc688wvmtez9t767r6jxu569e09vhaycx9v6hh689dd3jvs5qqrd7slr;

        record AccessRecord {
            owner:address,
        }

        struct TokenMetadata {
            token_id: field,
            name: u128,
            symbol: u128,
            decimals: u8,
            supply: u128,
            max_supply: u128,
            admin: address,
            external_authorization_required: bool,
            external_authorization_party: address
        }

        struct BeneficiaryMetadata {
            block_duration: u32,
            start_height: u32,
            number_of_interval: u32,
            total_amount: u128,
            claimed_amount: u128,
            token_id: field,  
        }
        mapping beneficiary_metadata:address => BeneficiaryMetadata;

        transition issue_access_record (to:address) -> AccessRecord {
            assert_eq(self.caller, ADMIN);
            return AccessRecord {
                owner: to
            };
        }


        mapping registered_token:field => bool;

        async transition register_token (token_id:field) -> Future {
            assert_eq(self.caller,ADMIN);
            return finalize_register_token(token_id);
        }

        async function finalize_register_token (token_id:field) {
            let token:TokenMetadata = token_registry.aleo/registered_tokens.get(token_id);
            let registered: bool = registered_token.get_or_use(token_id,false);
            assert(!registered);
            registered_token.set(token_id,true);
        }

        async transition deposit(beneficiary:address,amount:u128, block_duration:u32, start_height:u32, number_of_interval:u32, token_id:field ) -> Future {
            assert_eq(self.caller, ADMIN);
            let deposited:Future=token_registry.aleo/transfer_public(token_id, self.address, amount);
            let beneficiary_hash_address:address = BHP256::hash_to_address(beneficiary);
            return finalize_deposit(amount, beneficiary_hash_address , block_duration, start_height, number_of_interval,token_id,  deposited);
        }

        async function finalize_deposit (amount:u128, beneficiary: address, block_duration:u32, start_height:u32, number_of_interval:u32, token_id:field, deposited:Future) {
            deposited.await();
            let token:bool = registered_token.get(token_id);
            let metadata:BeneficiaryMetadata = beneficiary_metadata.get_or_use(beneficiary, BeneficiaryMetadata {
                block_duration: 0u32,
                start_height: 0u32,
                number_of_interval: 0u32,
                total_amount: 0u128,
                claimed_amount: 0u128,
                token_id: token_id
            });
            let total_amount:u128 = metadata.total_amount + amount;
            assert(start_height > block.height);
            beneficiary_metadata.set(beneficiary, BeneficiaryMetadata {
                block_duration: block_duration,
                start_height: start_height,
                number_of_interval: number_of_interval,
                total_amount: total_amount,
                claimed_amount: metadata.claimed_amount,
                token_id: token_id
            });
        
        }


        async transition claim(access_record: AccessRecord, amount:u128, token_id: field, external_authorization_required: bool) -> (token_registry.aleo/Token, AccessRecord,Future) {
            let (token_record,transfer):(token_registry.aleo/Token, Future)=token_registry.aleo/transfer_public_to_private(token_id, access_record.owner, amount, external_authorization_required);
            let beneficiary_hash_address:address = BHP256::hash_to_address(access_record.owner);
            let new_access_record:AccessRecord = AccessRecord {
                owner: access_record.owner
            };
            return (token_record, new_access_record, finalize_claim(amount, token_id, beneficiary_hash_address, transfer));
        }

        async function finalize_claim (amount:u128, token_id:field, beneficiary:address, transfer:Future) {
            transfer.await();
            let metadata:BeneficiaryMetadata = beneficiary_metadata.get(beneficiary);
            let claimed_amount:u128 = metadata.claimed_amount + amount;
            assert(claimed_amount <= metadata.total_amount);
            assert(block.height > metadata.start_height);
            assert((block.height - metadata.start_height)> (metadata.block_duration / metadata.number_of_interval));
            assert(amount == (metadata.total_amount * metadata.number_of_interval as u128 / metadata.block_duration as u128));
            beneficiary_metadata.set(beneficiary, BeneficiaryMetadata {
                block_duration: metadata.block_duration,
                start_height: metadata.start_height,
                number_of_interval: metadata.number_of_interval,
                total_amount: metadata.total_amount,
                claimed_amount: claimed_amount,
                token_id: metadata.token_id
            });
        }

    }
    ~~~

    </blockquote></details>

## Deploy Project:
- Command:
    ```sh
    leo deploy --network testnet -y
    ```
    <details><summary> Detailed Output </summary><blockquote>

    ~~~sh

       Leo ✅ Compiled 'gcsita_token_vesting_linear.aleo' into Aleo instructions
    📦 Creating deployment transaction for 'gcsita_token_vesting_linear.aleo'...


    Base deployment cost for 'gcsita_token_vesting_linear.aleo' is 13.1082 credits.

    +----------------------------------+----------------+
    | gcsita_token_vesting_linear.aleo | Cost (credits) |
    +----------------------------------+----------------+
    | Transaction Storage              | 5.326000       |
    +----------------------------------+----------------+
    | Program Synthesis                | 6.782200       |
    +----------------------------------+----------------+
    | Namespace                        | 1.000000       |
    +----------------------------------+----------------+
    | Priority Fee                     | 0.000000       |
    +----------------------------------+----------------+
    | Total                            | 13.108200      |
    +----------------------------------+----------------+

    Your current public balance is 14.537062 credits.

    ✅ Created deployment transaction for 'gcsita_token_vesting_linear.aleo'

    Broadcasting transaction to https://api.explorer.provable.com/v1/testnet/transaction/broadcast...

    ⌛ Deployment at18sxa27g93m9hu5fjfrrvu8qc3hqzxv33uyum9pv4p956wh5rfcgs90h2fk ('gcsita_token_vesting_linear.aleo') has been broadcast to https://api.explorer.provable.com/v1/testnet/transaction/broadcast.
    ~~~

    </blockquote></details>

- On-Chain Outputs:
    - Transaction ID: `at18sxa27g93m9hu5fjfrrvu8qc3hqzxv33uyum9pv4p956wh5rfcgs90h2fk`
    - Aleo Program: [https://testnet.aleo.info/program/gcsita_token_vesting_linear.aleo](https://testnet.aleo.info/program/gcsita_token_vesting_linear.aleo)
    - Deploy Txn: [https://testnet.aleo.info/transaction/at18sxa27g93m9hu5fjfrrvu8qc3hqzxv33uyum9pv4p956wh5rfcgs90h2fk](https://testnet.aleo.info/transaction/at18sxa27g93m9hu5fjfrrvu8qc3hqzxv33uyum9pv4p956wh5rfcgs90h2fk) 


# Signature:
## Sign with `Transaction ID`:
- For me, program deployed Transaction ID is: `at18sxa27g93m9hu5fjfrrvu8qc3hqzxv33uyum9pv4p956wh5rfcgs90h2fk`. Command:
    ```sh
    leo account sign -d --private-key <redacted> --message "at18sxa27g93m9hu5fjfrrvu8qc3hqzxv33uyum9pv4p956wh5rfcgs90h2fk" --raw
    ```
- Output:
    ```sh
    sign1ln6fw046w8uvulvg5nec36y9fzmyne40kj04yyzr3tjcqav0wyq98ldg2c7wv9vnsjdn6jdp0t60vvrf4zjcv6pes6dqyzqyn596vqnu23rr6mhkgkrusjsmmc7taewueergms2yf25r2fxqy9fphr4aqrj4uvr5pmmj9hcjx0g5kvp3mxavfe7fq5ppdgjnkpyg65u0acrscqvcwdk
    ```