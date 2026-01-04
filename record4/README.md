Record #4, claude streaming in a container

For certain reasons, I needed to run Claude in a container in headless mode. For debugging and observability, it is
convenient to see what is going on in the middle of execution, not only the final response.

At first glance, this looks easy to set up, but in practice it took several iterations before everything worked as
expected.

Issues I ran into:

- Claude CLI does not indicate that --verbose is mandatory when using --output-format stream-json
- jq requires --unbuffered, otherwise streaming output is buffered and nothing is shown in real time

```
claude \
    --verbose \
    --output-format stream-json \
    -p "$PROMPT" 2>&1 | jq --unbuffered -r '
      select(.type == "assistant")
      .message.content[]?
      | .text
        // (
          select(.input.command)
          | "\(.name): \(.input.description)"
        )
    '
```

#records #claude