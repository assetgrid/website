---
layout: ../../layouts/LayoutDocs.astro
title: "Assetgrid | Reference documentation"
---

# API

Everything you can do through the user interface can also be done using the API.
You can find an automatically generated API reference by going to the following url. Replace Assetgrid with the url of your own installation.

    assetgrid/swagger/index.html

Assetgrid user JWT bearer token authentication. You generate a token by signing in with the endpoint "/api/v1/User/Authenticate".

Once you have a token, provide the following header to access endpoints requiring authentication:

    Authorization: Bearer: <your token>