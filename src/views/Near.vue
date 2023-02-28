<Vue>
 async fetchValidatorInfo() {
            try {
                const apiKey = "34bfa5bc-eced-4954-aac5-bf36aaeabcde";
                const url = "https://near.getblock.io/mainnet";
                const headers = {
                    "x-api-key": apiKey,
                    "Content-Type": "application/json"
                };
                const data = {
                    jsonrpc: "2.0",
                    method: "validators",
                    params: [null],
                    id: "getblock.io"
                };
                const response: AxiosResponse = await axios.post(url, data, { headers });
                console.log(response.data.result);
                const validators = response.data.result.current_validators;
                for (const validator of validators) {
                    if (validator.public_key === "ed25519:2kAo86DW8mDaLDg37rFhQY8UYSZVq1CtegUHBEDvpSMA") {
                        this.validatorInfo = {
                            stake: validator.stake,
                            num_expected_blocks: validator.num_expected_blocks,
                            num_expected_chunks: validator.num_expected_chunks,
                            num_produced_blocks: validator.num_produced_blocks,
                            num_produced_chunks: validator.num_produced_chunks,
                            validator_stake_struct_version: validator.validator_stake_struct_version
                        };
                        break;
                    }
                }
            }
            catch (error) {
                console.error(error);
            }
        },
</Vue>