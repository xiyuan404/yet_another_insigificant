## get ip address
For private/internal ipv4 address

  For Wi-Fi, enter `ipconfig getifaddr en0` and your local IP will appear.

  For wired connections, enter `ipconfig getifaddr en1` into the Terminal and your local IP will appear

For external/public ipv4 address `dig +short txt ch whoami.cloudflare @1.0.0.1`


## get formatted current day

```js
const today = new Date().toLocaleDateString("en-US", {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "2-digit",
});
console.log(today);
```

_all options_
```js
{
  weekday: 'narrow' | 'short' | 'long',
  era: 'narrow' | 'short' | 'long',
  year: 'numeric' | '2-digit',
  month: 'numeric' | '2-digit' | 'narrow' | 'short' | 'long',
  day: 'numeric' | '2-digit',
  hour: 'numeric' | '2-digit',
  minute: 'numeric' | '2-digit',
  second: 'numeric' | '2-digit',
  timeZoneName: 'shortOffset' | 'shortGeneric' | 'longOffset' | 'longGeneric',
  // Time zone to express it in
  timeZone: 'Asia/Shanghai',
  // Force 12-hour or 24-hour
  hour12: true | false,

  // Rarely-used options
  hourCycle: 'h11' | 'h12' | 'h23' | 'h24',
  formatMatcher: 'basic' | 'best fit'
}
```

```js
console.log(
  new Intl.DateTimeFormat("en-US", {
    weekday: "long",
    year: "numeric",
    month: "short",
    day: "2-digit",
    timeZone: "Asia/Shanghai",
    timeZoneName: "shortOffset",
  }).format(new Date())
);

```


## color system
```css
:root {

  --yellow: #fad956;
}
```
