# &lt;Avatar&gt;[![Build Status](https://travis-ci.org/Sitebase/react-avatar.svg?branch=master)](https://travis-ci.org/Sitebase/react-avatar)
Universal avatar makes it possible to fetch/generate an avatar based on the information you have about that user.
We use a fallback system that if for example an invalid Facebook ID is used it will try Google, and so on.

![React Avatar component preview](example1.jpg)

For the moment we support following types:

* Facebook
* Google
* Twitter (using [Avatar Redirect](#avatar-redirect))
* Instagram (using [Avatar Redirect](#avatar-redirect))
* Vkontakte
* Skype
* Gravatar
* Custom image
* Name initials

The fallbacks are in the same order as the list above were Facebook has the highest priority.

## Demo

[Check it live!](https://www.sitebase.be/react-avatar/?utm_source=github&utm_medium=readme&utm_campaign=react-avatar)

## Install

Install the component using [NPM](https://www.npmjs.com/):

```sh
$ npm install react-avatar --save

# besides React, react-avatar also has react-addons-shallow-compare and prop-types
# as peer dependencies, make sure to install the correct version
# for your version of react
$ npm install react-addons-shallow-compare@^0.14 --save
# OR
$ npm install react-addons-shallow-compare@^15 --save
```

Or [download as ZIP](https://github.com/sitebase/react-avatar/archive/master.zip).


## Usage

1. Import Custom Element:

    ```js
    import Avatar from 'react-avatar';
    ```

2. Start using it!

    ```html
    <Avatar name="Foo Bar" />
    ```

**Some examples:**

```html
<Avatar googleId="118096717852922241760" size="100" round={true} />
<Avatar facebookId="100008343750912" size="150" />
<Avatar vkontakteId="1" size="150" />
<Avatar skypeId="sitebase" size="200" />
<Avatar twitterHandle="sitebase" size="40" />
<Avatar name="Wim Mostmans" size="150" />
<Avatar name="Wim Mostmans" size="150" textSizeRatio="1.75" />
<Avatar value="86%" size="40" />
<Avatar size="100" facebook-id="invalidfacebookusername" src="http://www.gravatar.com/avatar/a16a38cdfe8b2cbd38e8a56ab93238d3" />
<Avatar name="Wim Mostmans" unstyled="true" />
```

**Manually generating a color:**

```html
import Avatar from 'react-avatar';

<Avatar color={Avatar.getRandomColor('sitebase', ['red', 'green', 'blue'])} name="Wim Mostmans" />
```

**Configuring React Avatar globally**

```html
import Avatar, { ConfigProvider } from 'react-avatar';

<ConfigProvider colors={['red', 'green', 'blue']}>
    <YourApp>
        ...
        <Avatar name="Wim Mostmans" />
        ...
    </YourApp>
</ConfigProvider>

```

## Options

### Avatar

|   Attribute   |      Options      | Default |                                              Description                                               |
| ------------- | ----------------- | ------- | ------------------------------------------------------------------------------------------------------ |
| `className`   | *string*          |         | Name of the CSS class you want to add to this component alongside the default `sb-avatar`.             |
| `email`       | *string*          |         | String of the email address of the user.                             |
| `md5Email` | *string* |         | String of the MD5 hash of email address of the user. |
| `facebookId` | *string* |         |                                                                                                        |
| `twitterHandle` | *string* |         |                                                                                                        |
| `instagramId` | *string* |         |                                                                                                        |
| `googleId`   | *string*             |         |                                                                                                        |
| `skypeId`    | *string*          |         |                                                                                                        |
| `name`        | *string*          |         | Will be used to generate avatar based on the initials of the person                                    |
| `maxInitials` | *number*          |         | Set max nr of characters used for the initials. If maxInitials=2 and the name is Foo Bar Var the initials will be FB  |
| `value`       | *string*          |         | Show a value as avatar                                                                                 |
| `color`       | *string*          | random  | Used in combination with `name` and `value`. Give the background a fixed color with a hex like for example #FF0000 |
| `fgColor`     | *string*          | #FFF  | Used in combination with `name` and `value`. Give the text a fixed color with a hex like for example #FF0000 |
| `size`        | *[length][1]*             | 50px      | Size of the avatar                                                                                     |
| `textSizeRatio` | *number*             | 3      | For text based avatars the size of the text as a fragment of size (size / textSizeRatio)                                 |
| `round`       | *bool or [length][1]*            | false   | The amount of `border-radius` to apply to the avatar corners, `true` shows the avatar in a circle.          |
| `src`         | *string*          |         | Fallback image to use                                                                                  |
| `style`         | *object*          |         | Style that will be applied on the root element                                                       |
| `unstyled`    | *bool*            | false   | Disable all styles                                                                                     |
| `onClick`    | *function*            |        | Mouse click event                                                                                     |

### ConfigProvider

|   Attribute   |      Options      | Default |                                              Description                                               |
| ------------- | ----------------- | ------- | ------------------------------------------------------------------------------------------------------ |
| `colors`       | *array(string)*  | [default colors](https://github.com/Sitebase/react-avatar/tree/master/src/utils.js#L39-L47)  | A list of color values as strings from which the `getRandomColor` picks one at random. |
| `cache`       | *[cache](#implementing-a-custom-cache)*          | [internal cache](https://github.com/Sitebase/react-avatar/tree/master/src/cache.js)  | Cache implementation used to track broken img URLs |
| `avatarRedirectUrl`  | *URL*          | `undefined`  | Base URL to a [Avatar Redirect](#avatar-redirect) instance |


### Avatar Redirect

[Avatar Redirect][2] adds support for social networks which require a server-side service to find the correct avatar URL.

Examples of this are:

- Twitter
- Instagram

An open Avatar Redirect endpoint is provided at `https://avatar-redirect.appspot.com`. However this endpoint is provided for free and as such an explicit opt-in is required as no guarantees can be made about uptime of this endpoint.

Avatar Redirect is enabled by setting the `avatarRedirectUrl` property on the [ConfigProvider context](#configprovider)

## Development

In order to run it locally you'll need to fetch some dependencies and a basic server setup.

* Install local dependencies:

    ```sh
    $ npm install
    ```

* Auto build new test version when developing that can be run with `grunt connect`:

    ```sh
    $ npm run dev
    ```

* To test your project, start the development server and open `http://localhost:8000/index.html`.

    ```sh
    $ python -m SimpleHTTPServer
    ```
    
### Implementing a custom cache

`cache` as provided to the `ConfigProvider` should be an object implementing the methods below. The default cache implementation can be found [here](https://github.com/Sitebase/react-avatar/tree/master/src/cache.js)

|   Method   |                                              Description                                               |
| ------------- | ------------------------------------------------------------------------------------------------------ |
| `set(key, value)`                 | Save `value` at `key`, such that it can be retrieved using `get(key)`. Returns `undefined` |
| `get(key)`                        | Retrieve the value stored at `key`, if the cache does not contain a value for `key` return `null` |
| `sourceFailed(source)`            | Mark the image URL specified in `source` as failed. Returns `undefined` |
| `hasSourceFailedBefore(source)`   | Returns `true` if the `source` has been tagged as failed using `sourceFailed(source)`, otherwise `false`. |

## Products using React Avatar
* [BuboBox](https://www.bubobox.com/?utm_source=github&utm_medium=readme&utm_campaign=react-avatar)
* [Ambassify](https://www.ambassify.com/?utm_source=github&utm_medium=readme&utm_campaign=react-avatar)

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## History

For detailed changelog, check [Releases](https://github.com/sitebase/react-avatar/releases).

## Maintainers

 - [@Sitebase](https://github.com/Sitebase) (Creator)
 - [@JorgenEvens](https://github.com/JorgenEvens)

## License

[MIT License](http://opensource.org/licenses/MIT)

[1]: https://developer.mozilla.org/en-US/docs/Web/CSS/length
[2]: https://github.com/JorgenEvens/avatar-redirect