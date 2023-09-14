## Microsoft / Windows / Licensing

### Determine whether a device has an embedded OEM Product Key

```pwsh
(Get-CimInstance -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
```
