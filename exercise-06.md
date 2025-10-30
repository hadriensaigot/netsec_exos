# Exercise sheet 06: TLS Part 1 (28 points) - *23 October 2025*

Handing in this exercise sheet is optional.
If you want individual feedback for your solutions, you have to hand in your solution by the **Wednesday following exercise publication, 29 October 2025, at 18:00**.
The hand-in procedure is as follows:

- copy this document, and answer the questions in the appropriate spaces;
- create a new issue on [the GitLab issues repo](None);
- the issue title must be in the form `Exercise 06/TLS: {YOUR NETHZ ID}` (without curly braces);
- set the issue as _confidential_;
- the "Exercise Hand-In" label must be added to the issue;
- paste the modified document with your solution in the body of the issue.


### Question 1 (2 points)
> As we have learnt in the last lecture, the TLS 1.3 record protocol offers an
> optional padding feature to hide the true length of fragments. Can you think of
> a possible way to recover the true length of the fragments? Think of the way
> TLS implementations might remove the padding. Could it reveal something?
> (Note: it’s not a padding oracle)

_Solution_:
If the padding vector is known to everyone, then one can infer the true length 
of the message sent by substracting the received message length and the padding vector length. A way TLS might remove padding is to implement the ability to sent segments with variable lengths.

### Question 2 (6 points)
In the TLS 1.3 record protocol, the per-record nonce is based upon a
64-bit counter.

**2.1.** (2 points)
> The counter always starts at 0. Why is this not a problem?

_Solution_:
It is not a problem to have a per record nonce based upon a counter which 
always starts at 0 because the nonce is processed in the AEAD so an attacker 
cannot infer what number the counter is at.


**2.2.** (2 points)
> Given an average record length of 1000 bytes and a modern connection
> speed of 1Gb/s, and ignoring the fact that any sane TLS implementation
> would rekey and update the IV earlier[1], how many years would it take to
> reset the counter back to 0?
> 
> [1]: https://datatracker.ietf.org/doc/html/rfc8446#section-7.2

_Solution_:
Your solution here...

**2.3.** (2 points)
> What would happen if some records were to be received out of order?

_Solution_:
If some records are received out of order the server side TLS handler will close the connection because the out of order record won't have the expected counter number in the NONCE after decoding.

### Question 3 (4 points)
TLS 1.3 has fixed several vulnerabilities and design problems that plagued
previous versions of the protocol. It also greatly reduced the complexity of
some key aspects of the protocol.

**3.1.** (2 points)
> In TLS 1.2, cipher suites were represented as, for example:
> 
> ```
> TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
> ```
> 
> In TLS 1.3, the same cipher suite is instead represented as:
> 
> ```
> TLS_AES_128_GCM_SHA256
> ```
> 
> Clearly, some information has been removed. What is missing in the new
> representation? Can you speculate (or find out) why this information was
> removed?

_Solution_:
In TLS 1.3 the cipher suite no longer specifies the key exchange algorithm and the  signature authentication algorithm. This information was removed because we no longer have different options for key exchange and authentication process, all connections uses the most secured one.

**3.2.** (2 points)
> The total number of accepted cipher suites, key exchanges, and signature
> algorithms has also been greatly reduced. What are the security advantages
> of this approach?

_Solution_:
This approach prevent users from using outdated algorithms proned to attacks (more robust). It also reduces the attack space as less algorithms are given as options. Finally, forward secrecy is achieved because it used ECDHE by default. 

### Question 4 (6 points)
At USENIX '13, AlFardan, Bernstein, Paterson, Poettering, and Schuldt
presented an attack on the usage of the RC4 stream cipher in TLS version
1.2 and below. (Note that RC4 has since been prohibited in RFC7465 for
the affected versions).

The attack exploits probability biases in the bytes of the RC4 keystream,
where certain bytes are more common in certain positions (single-byte bias
attack). The authors also present a variant exploiting biases in
consecutive bytes (double-byte bias attack).

The attack allows them to *fully* recover the TLS-protected plaintext,
given enough repeated encryptions of the same data.

This information is sufficient to solve the questions below, but for those
who are curious, the paper below explains the mechanism in depth (and
may make it easier to answer questions):

["On the Security of RC4 in TLS"](https://www.usenix.org/conference/usenixsecurity13/technical-sessions/paper/alFardan)

**4.1.** (4 points)
> You want to implement this attack, targeting a secure cookie in the HTTPS
> session between a client running a web browser *B* and a web server *W*.
> 
> How could you force the browser to transmit the same data multiple
> times? You don’t have direct control of the browser nor of the
> web server, but you know *W* uses secure cookies over HTTPS, and you
> control a second web server, *E*, which the client visits.
> 
> Hint: The authors of the paper were able to perform this! Perhaps they
> have written the methods down in their Validation sections?

_Solution_:
Your solution here...

**4.2.** (2 points)
> Single-byte biases in the RC4 cipher are stronger at the start of the
> keystream. Using this information, propose a possible mitigation for the
> attack.

_Solution_:
Your solution here...

### Question 5 (5 points)
One famous OpenSSL attack, Heartbleed [1], exploits some server
implementations of keep-alive (heartbeat) messages. These messages can
be sent even before the handshake is complete. They are meant to check
if the other side is still online and verify that data transfer works
correctly. One side sends a buffer and requests a certain amount of
characters from that buffer. Unfortunately, the vulnerable servers had a
missing bounds check...

[1] <https://heartbleed.com/>

**5.1.** (2 points)
> Let’s assume that your website uses login cookies (e.g., string
> LOGIN\_COOKIE=/16 byte value/) and stores recent request data in nearby
> memory locations. How would you use Heartbleed to hijack a session of
> someone who logged in recently?

_Solution_:
Your solution here...

**5.2.** (1 points)
> Does client authentication (e.g., using client certificates) offer
> protection against Heartbleed?

_Solution_:
Your solution here...

**5.3.** (2 points) (bonus)
> Let’s assume that you were using TLS 1.2 with ciphersuite `TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA384`,
> and the server you connected to was vulnerable to Heartbleed.
> Can an attacker who recorded your traffic decrypt it later?
> What if you had used any of the ciphersuites of TLS 1.3?

_Solution_:
Your solution here...

### Question 6 (5 points)
The Heartbleed attack we have just seen belongs to the category of
implementation bug exploits. The TLS protocol has had, during the years,
several of these attacks, which have been solved by simply patching
software libraries and servers.

**6.1.** (2 points)
> In 2014, Apple discovered an implementation error in SecureTransport,
> the TLS library used in iPhones and Macs. What is the problem with this
> C code?
> 
> ```c
> static OSStatus SSLVerifySignedServerKeyExchange (SSLContext *ctx, bool isRsa,
>         SSLBuffer signedParams, uint8_t *signature, UInt16 signatureLen) {
>     OSStatus err;
>     ...
>     if ((err = SSLHashSHA1.update(&hashCtx, &serverRandom)) != 0)
>             goto fail;
>     if ((err = SSLHashSHA1.update(&hashCtx, &signedParams)) != 0)
>             goto fail;
>             goto fail;
>     if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)
>             goto fail;
>     ...
>     err = sslRawVerify(...);
> fail:
>         SSLFreeBuffer(&signedHashes);
>         SSLFreeBuffer(&hashCtx);
>         return err;
> }
> ```

_Solution_:
Your solution here...

**6.2.** (3 points)
> How might an attacker take advantage of this bug? You can find a hint
> in the method signature.

_Solution_:
Your solution here...
