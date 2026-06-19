# SVX 1.3.3 Release Notes

**Software Release Date**: March 15, 2024

**Summary**:

This release introduces some changes in the VC API component that aim to work with the `vc+sd-jwt`  format.  

`vp_formats` verifier client metadata for `vc+sd-jwt` follows the specification and includes `sd-jwt_alg_values` and `kb-jwt_alg_values` information.
Here is an example:
```json
 {
   "vp_formats": {
     "vc+sd-jwt": {
      "sd-jwt_alg_values": [
         "ES256", "EdDSA"
       ],
      "sd-jwt_alg_values": [
         "ES256", "EdDSA"
       ]
     }
 }
```

## Deprecations and EOL

None
