# TBS (Turn based strategy)

Ok I think I possibly have an idea of how I'm going to do the turn based strategy game.

1. Have the data stored in dynamodb. serve via lambda
2. Use unity (or anything really) to render out that JSON file (have lambda serve)
3. User makes some moves on the unity game
4. Validate those are valid changes (lambda)
5. Update JSON file with new changes and send next user a notification. (lambda)
6. Run some time based functions to remind or skip a person

Pros:
- I can probably get it running relatively fast
- Can probably have a historical view
- hella cheap (no constantly running server)

Cons:
- if I'm sending full file, that's possibly a lot of data, it also allows users to see the map so no such thing as  fog of war 
    - possibly only figure out all the actions user can take


PS if you see this repo and see any possible problems I maybe overlooking feel free to create an issue 

## Goals
- have it run entirely on aws free tier(always free). so that others can also setup a game for their friends. This means:
    - S3 storage < 5GB 
    - <1M lambda / month (I believe this should be more than enough)
    - dynamodb < 25GB (might be a better option than S3)

- if there is a web front-end: use ghpages

## Architecture Items 
- lambdas:
    - ServeGame
        - send renderable file
    - UpdateGame
        - validate
        - update gamestate
    - Notify
        - send notification email
        - notification email has unique code/link that allows for updating
            - can use something like current hash(current game state + useremail (not sure if that can be iterated/guessed) )
        - skip a turn (possibly need cloudwatch)
    - CreateGame
        - based on useremails, create a game
        - generate a link to view current state of game
    - EndGame / Winstate
        - Delete the resources required to run the game


