# Olmofe Ansible + Laravel Demo

This repository is a fairly modified repo of the [Automating Server Setup - a DigitalOcean Workshop Kit](https://www.digitalocean.com/community/meetup_kits/automating-server-setup-with-ansible-a-digitalocean-workshop-kit) and [laravel real world example](https://github.com/f1amy/laravel-realworld-example-app).

URL: [http://exam.olmofe.live/](http://exam.olmofe.live/)

## Setup

TL;DR:

Make sure you have [Ansible installed](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04) on your control node, which can be your local machine or a remote server dedicated to running Ansible.
The playbooks is verified for Ubuntu 18.04+ server to serve as node and host.

1. Clone this repository
2. edit the hosts file to your customer slave/php-server.You can also add for production(sample in inventory file `inventory-example`)
3. Adjust values on your `group_vars/all.yml` file.
The remote_user variable should be an existing can with sudo access, otherwise, you must launch ansible with -u root and uncomment lines in setup role that creates sudo user.
- -  if using existing remote_user, you should copy ansible server id_rsa.pub file (from wherever it is on the machine probably `~/.ssh`) into the authorized keys of the ansible slave/php server (probably founnd in `~/.ssh/authorized_keys`)
3. You can check if the ansible server is able using a ping eg. `ansible all -i hosts -m ping -u sudo-user`

4. Run the `server-setup.yml` playbook to set up the LEMP server
eg. `ansible-playbook -i hosts laravel-deploy.yml -u sudo-user`
5. Run the `laravel-deploy.yml` playbook to deploy the demo Laravel application.
6. Access your server's IP address or chosen hostname( same as http_host value in group vars ans should be set as an A record with your domain provider ) to test the setup.

6. swagger is not included in the app, to test endpoints see [here](https://github.com/gothinkster/realworld/tree/main/api)
7. Here are the routes in this demo: 

```
GET|HEAD  / ................................................................................. 
  POST      api/articles .......... api.articles.create › Api\Articles\ArticleController@create
  GET|HEAD  api/articles .............. api.articles.list › Api\Articles\ArticleController@list
  GET|HEAD  api/articles/feed ......... api.articles.feed › Api\Articles\ArticleController@feed
  PUT(i think this should be POST)       api/articles/{slug} ... api.articles.update › Api\Articles\ArticleController@update
  DELETE    api/articles/{slug} ... api.articles.delete › Api\Articles\ArticleController@delete
  GET|HEAD  api/articles/{slug} ........ api.articles.get › Api\Articles\ArticleController@show  
  POST      api/articles/{slug}/comments api.articles.comments.create › Api\Articles\CommentsC…
  GET|HEAD  api/articles/{slug}/comments api.articles.comments.get › Api\Articles\CommentsCont…  
  DELETE    api/articles/{slug}/comments/{id} api.articles.comments.delete › Api\Articles\Comm…  
  POST      api/articles/{slug}/favorite api.articles.favorites.add › Api\Articles\FavoritesCo…  
  DELETE    api/articles/{slug}/favorite api.articles.favorites.remove › Api\Articles\Favorite…  
  GET|HEAD  api/documentation . l5-swagger.default.api › L5Swagger\Http › SwaggerController@api  
  GET|HEAD  api/oauth2-callback l5-swagger.default.oauth2_callback › L5Swagger\Http › SwaggerC…  
  GET|HEAD  api/profiles/{username} ............. api.profiles.get › Api\ProfileController@show  
  POST      api/profiles/{username}/follow . api.profiles.follow › Api\ProfileController@follow  
  DELETE    api/profiles/{username}/follow api.profiles.unfollow › Api\ProfileController@unfol…  
  GET|HEAD  api/tags .................................. api.tags.list › Api\TagsController@list  
  GET|HEAD  api/user .............................. api.users.current › Api\UserController@show  
  PUT       api/user ............................. api.users.update › Api\UserController@update
  POST      api/users ........................ api.users.register › Api\AuthController@register  
  POST      api/users/login ........................ api.users.login › Api\AuthController@login  
  GET|HEAD  docs/asset/{asset} l5-swagger.default.asset › L5Swagger\Http › SwaggerAssetControl…  
  GET|HEAD  docs/{jsonFile?} l5-swagger.default.docs › L5Swagger\Http › SwaggerController@docs   
  GET|HEAD  hello .............................................................................  
  GET|HEAD  sanctum/csrf-cookie ................... Laravel\Sanctum › CsrfCookieController@show
  ```