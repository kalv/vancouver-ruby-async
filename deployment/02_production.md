!SLIDE smbullets
# Some tips

- You can set `appendonly yes` on Redis to buffer write out redis keys
- If you manually require a restart on redis play with `save` from
  `redis-cli`
- Use ids as arguments and not objects in case objects change when being
  processed
