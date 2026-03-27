# lila-tictactoe

## Railway manual start command (backend)

If the service keeps starting Nakama with default DB settings (`root@localhost:26257`), set the Railway **Start Command** manually to:

```bash
/nakama/entrypoint.sh
```

This forces startup through the custom script that resolves DB env vars, runs migrations, and then launches Nakama.
