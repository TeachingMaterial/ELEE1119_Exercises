# Introduction

Once you have managed to get into the Debain OS on the Rock C+, you will need to do a couple of steps to get the OS ready.

1. `sudo apt update`
    - This should update the system however you may get a response that says:
    ```sh
    > W: An error occured during the signature verifcation. The repository is not updated and the previous index files will be used. GPG error: http://apt.radxa/com/bullseye-stable bullseye InRelease: The following signatures could not be verified because the public key is not available: NO_PUBKEY 9B98116C9AA302C7
    > W: Filed to fetch http://apt.radxa/com/bullseye-stable/dists/bullseye/InRelease The following signatures could not be verified because the public key is not available: NO_PUBKEY 9B98116C9AA302C7
    > W: Some index files failed to download. They have been ignored, or old ones used instead.
    ```
    -  If that is the case you will need to run the following command:
    ```sh
    $ wget -O - apt.radxa.com/focal-stable/public.key | sudo apt-key add
    ```

    - You should get some feedback with a HTTP request and a Warning about apt-key is deprecated... this is fine (if only there was a deterministic, declarative system out there we could have used...)

    - Run the `sudo apt update` again and once finished run `sudo apt upgrade` this my take 10 mins.

    Source: [https://wiki.radxa.com/Rock5/linux/radxa-apt#focal-stable](https://wiki.radxa.com/Rock5/linux/radxa-apt#focal-stable)

