# Cheatsheets • Microsoft • Coding • PowerShell

## Networking
**Send an HTTP `GET` Request**
```powershell
# Definining `Method`, `Headers` and `Uri` params.
Invoke-RestMethod -Method GET -Headers @{ 'Accept' = 'application/json' } -Uri 'https://api.github.com/users/johngagefaulkner'

# HTTP Method is `GET` by default and most REST APIs return `JSON` by default so those params can be excluded.
# The following command will provide the same result as the first command. This functions similarly to CURL.
Invoke-RestMethod https://api.github.com/users/johngagefaulkner
```

