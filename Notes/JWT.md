## JWT(JSON web tokens)
Links : 
- [ChaiAndCode](https://www.youtube.com/watch?v=xrj3zzaqODw)
- [Detailed JWT Sections](https://www.youtube.com/watch?v=Y4joeekt5Ew)
- [Docs](https://jwt.io/)


### Token structure
- Header :    
    - Signing algorithm
    - the type of the token
     ```JSON 
        {
        //Base64Url encoded 
        "alg": "HS256", // HMAC SHA256 or RSA etc.
        "typ": "JWT"
        }
    ```
- Payload(Claims) : 
    - Claims : 
        
        Claims are statements about an entity (typically, the user) and additional data.
        - Registered claims : set of predefined claims which are not mandatory but recommended
            - iss (Issuer): Identifies the principal that issued the token.
            - sub (Subject): Identifies the principal that is the subject of the token.
            - aud (Audience): Identifies the recipients that the token is intended for.
            - exp (Expiration Time): The time after which the token expires.
            - nbf (Not Before): The time before which the token must not be accepted.
            - iat (Issued At): The time at which the token was issued.
            - jti (JWT ID): A unique identifier for the token.
        - Public claims : 
        
            These are custom claims that can be defined by anyone and should be collision-resistant. It is advisable to define these claims in the IANA JSON Web Token Registry or use namespaced claims to avoid conflicts.
        - Private claims : 

            These are custom claims created to share information between parties that agree on them. They are neither registered nor public claims.

            ```JSON
            {
              "sub": "1234567890",
              "name": "John Doe",
              "iat": 1516239022
            }

            ```
- Signature :

    To create the signature part, you need to have the encoded header, encoded payload, and a secret key (for HMAC algorithms) or a private key (for RSA or ECDSA algorithms). The process is as follows:

    - Encode the header and payload using Base64Url encoding.
    - Concatenate the encoded header and payload with a period (.).
    - Use the signing algorithm and the secret or private key to sign this string.
    - Syntax :
    ```C#
    //Encrypt is encryption function which encrypts the message with secret Key
    var signature = Encrypt(Hash(header+"."+payload));
    var secret = signature;
    // HMACSHA256 is encryption algo
    var jwt =  HMACSHA256(header+"."+payload+"."+secret);
    ```
    ```JSON
    HMACSHA256(
        base64UrlEncode(header) + "." +
        base64UrlEncode(payload),
        secret)

    ```
    - Eample : 
    ```JSON
    HMACSHA256(
        base64UrlEncode(header) + "." +
        base64UrlEncode(payload),
        "your-256-bit-secret")

    ```
- JWT token : 
``` Header.Payload.Signature ```
- Example : 
    ```JSON
     eyJhbGciOiAiSFMyNTYiLCAidHlwIjogIkpXVCJ9.eyJzdWIiOiAiMTIzNDU2Nzg5MCIsICJuYW1lIjogIkpvaG4gRG9lIiwgImlhdCI6IDE1MTYyMzkwMjJ9.lmFmoaR6p0bXN8rRH9Rbb14s7gD7URKwqVq2NTjsF1I
    ```