# LogComex Backend

Here is the Backend for user registration as well as authentication, based on Oauth.

These are the available endpoint:

`
POST | http://www.logcomexlogin.local/api/register
POST | http://www.logcomexlogin.local/oauth/token
`

## Instalation

- Clone the repo using git command: `git clone git@github.com:upcesar/logcomex-oauth2.git`. Project files are copied into the new folder `logcomex-oauth2`.
- Create a new Homestead VM. For further instructions about creating the environment: https://laravel.com/docs/5.4/homestead
- Once downloaded the Homestead VM, edit the file `src/stubs/Homestead.yaml` to setup folders, sites and databases
`
folders:
    - map: ~/my/laravel/folders
      to: /home/vagrant/Code

sites:
    - map: logcomexlogin.local
      to: /home/vagrant/Code/my/laravel/folders/logcomex-oauth2/public
      
    - map: www.logcomexlogin.local
      to: /home/vagrant/Code/my/laravel/folders/logcomex-oauth2/public
    

databases:
    - logcomex
`
- Execute `vagrant ssh`
- In the VM (guest) go to the folder `cd ~/Code/logcomex-oauth2` and then `artisan migrate && artisan passport:client --password`
- When tables related with oath are created, you will be prompted "What should we name the password grant client?". Type the desired name or just leave it blank and press Enter.
- Finally, you will get the "client_id" and "client_secret". These informations are stored in "oauth_clients" table.

## The endpoints

**Register**

- `/api/register` for creating new users. Use raw json using the POST verb as follows:
`
{
	"name" : "JOHN DOE",
	"email" : "johndoe@my-email.com",
	"password" : "test_password",
	"password_confirmation" : "test_password"
}
`

Parameters / Request:

- [name]  : User Full Name
- [email] : User's e-mail (login)
- [password] : User's e-mail (login)
- [password_confirmation] : Must be equal to [password] parameter.

Response:

If user is created with success, a JSON is rendered as follows:

`
{
    "status": "ok",
    "message": "usuário registrado com sucesso"
}
`

Otherwise:

`
{
    "status": "error",
    "errors": [
        "field1" : [
            "Error description 1", "Error description 2", ...
        ],
        "field2" : [
            "Error description 1", "Error description 2", ...
        ],
        ...
    ]
}
`

**Login**

- `/oauth/token` for authenticating user and get the ACCESS_TOKEN. Add to the header 'Content-Type : application/x-www-form-urlencoded' and 
`
client_id: 1
client_secret: QEYwMPMrGeBIzjHdrlI9QAt7L26ycgV8vV8S8Byu
grant_type:password
username:upcesar@gmail.com
password:123456
`

Parameters / Request:

- [client_id]  : Client ID generated. Default 1
- [client_secret] : Client Secret created on utility 
- [grant_type] : "password"
- [username] : User's E-Mail.
- [password] : User's password.

Response:

{
    "token_type": "Bearer",
    "expires_in": value in second,
    "access_token": "eyJ0eXAiOiJKV1Qi...", // Trimmed in this example
    "refresh_token": "def50200acdbb57..." // Trimmed in this example
}