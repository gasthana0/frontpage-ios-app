# Apollo iOS Hello World app

This is the simple example Apollo iOS app that lives at dev.apollodata.com.

## Installation

This project requires Xcode 8, which you can install from the [Mac App Store](https://itunes.apple.com/en/app/xcode/id497799835?mt=12).


You will have to install the `apollo-codegen` command globally through npm:

```sh
npm install -g apollo-codegen
```

- introspect schema

```
apollo-codegen introspect-schema http://localhost:8080/graphql --output schema.json
```

- generate api

```
apollo-codegen generate *.graphql --schema schema.json --output API.swift
```

### Server

This app talks to the frontpage example server, available here: https://github.com/apollostack/frontpage-server. Follow the instructions there and start the server before running the iOS app. (You can find the equivalent React app [here](https://github.com/apollostack/frontpage-react-app) if you want to run them side by side.)

## Starting the app

You can then open `FrontPage.xcodeproj` and press the run button to run the app. It should load a list of posts and display their titles, authors and number of votes in a table view. You can also upvote posts.

If you want to run on a device, change `localhost` to your machine's local IP address in `AppDelegate.swift`.

This is a very basic app, but it does demonstrate how you can hook up GraphQL query results to your UI. The code in `PostListViewController.swift` fetches data based on a GraphQL query defined in `PostListViewController.graphql`. That query refers to a fragment defined in `PostTableViewCell.graphql`, which nicely illustrates how you can describe your data needs next to the UI component that uses them.

Building the target will run `apollo-codegen` against all `.graphql` files in your project and generate query-specific result types in `API.swift`.

Try commenting out (GraphQL uses `#` for comments) a post's `title` and rebuild the target. You should get a compile-time error because the code in our view controller accesses `post.title`. The type system guarantees at compile-time that the data we access from our code is actually specified as part of the query.

`apollo-codegen` also validates GraphQL query documents against the schema, and Xcode will show validation errors inline. Try adding an `email` field under `author` for instance, and rebuild to show the errors.

![Xcode](/Screenshots/Xcode.png)
