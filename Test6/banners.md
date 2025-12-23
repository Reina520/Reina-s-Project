# Banners

The `banner` module contains functionality for displaying banners within your game.

!!! warning "Requirements for Advertisements"

    Please be sure to read our [advertisement requirements](/requirements/ads), since your game will be rejected without any feedback if it doesn't follow them.

## Getting started

After reading our [SDK Introduction](intro.md) page for your engine, access the `banner` module like this:

=== "HTML5"

    ```js
    window.CrazyGames.SDK.banner
    ```

=== "Unity"

    ```cs
    CrazySDK.Banner;
    ```

    !!! info

        For a demo, please consult the `CrazySDK/Demo/BannerModule` scene. You can run it directly in the Unity editor.

        When testing banners locally in your browser, please ensure you are using a WebGL template that stretches the game to cover the entire screen (for example the built-in PWA template). Otherwise, the position of the browser banners won't match your positioning in editor.

=== "GameMaker"

    Make sure to read the [introduction](/sdk/intro) page on setting up your project.

=== "Construct"

    ```js
    window.ConstructCrazySDK.banner;
    ```

=== "Godot"

    ```gd
    SDK.banner;
    ```

=== "Cocos"

    🟥 Not supported

## Request static banner

This paragraph explains how to request static banners. There are 5 banner sizes available:

-   Leaderboard (728x90)
-   Medium (300x250)
-   Mobile (320x50)
-   Main (468x60)
-   Large Mobile (320x100)

=== "HTML5"

    To begin, you need to have an HTML container of the banner size present on the screen:
    ```html
    <div id="banner-container" style="width: 300px; height: 250px"></div>
    ```
    And fill that using javascript:
    ```js
    try {
      // await is not mandatory when requesting banners,
      // but it will allow you to catch errors
      await window.CrazyGames.SDK.banner.requestBanner({
        id: "banner-container",
        width: 300,
        height: 250,
      });
    } catch (e) {
      console.log("Banner request error", e);
    }
    ```

=== "Unity"

    We provide a banner prefab, which you can find in `CrazySDK/Resources`. Drag the banner prefab into your scene.

    To change the banner size, modify the Banner size property of the newly created object.

    ![Banner size](/img/unity/change-banner-size.png)

    To change the banner position, select the `Banner` child of the newly created object and change its position.

    ![Banner position](/img/unity/change-banner-position.png)

    Showing and hiding the banners is done similiar to other Unity GameObjects, by calling the `SetActive(false)` or `SetActive(true)` method.

=== "GameMaker"

    You can request one or more banners using the `crazy_banner_request_banner()` function, with these arguments:

    - `banner_id` - integer
    - `width` - integer
    - `height` - integer
    - `position` - string

    ```gml
    crazy_banner_request_banner(
        0,
        300, 250,
        CRAZY_BANNER_POSITION.CENTER_MIDDLE,
        function() { show_debug_message("Static banner loaded!"); },
        function(err) { show_debug_message("Banner failed: " + json_stringify(err)); }
    );
    ```

=== "Construct"

    Request banners like this:

    ```js
    await window.ConstructCrazySDK.banner.requestBanners([
    {
        id: "main-menu-banner-1",
        width: 300,
        height: 250,
        x: 0,
        y: 0,
    },
    {
        id: "main-menu-banner-2",
        width: 300,
        height: 250,
        // display banner in right corner
        // 922 is layout width, 300 banner width
        x: 922 - 300,
        y: 0,
    },
    ]);
    ```

    The method takes a single argument which is an array of banners, so you can request also 1 banner for example. When calling the method, all the previous banners will be removed. If you need to refresh the banners, call the method again with the same banner list.

    You have to specify all 5 parameters for each banner:

    - `id:` a unique id to identify the banner
    - `x:` the horizontal position for the banner, from the bottom left screen corner
    - `y:` vertical position of the banner, from the bottom left screen corner
    - `width:` the banner width
    - `height:` the banner height

=== "Godot"

    The `banner` module contains functionality for displaying banners in game. To begin, you need to have an HTML container of the banner size present on the screen.
    To define a banner, open Project/Export and add another line to the `Head Include` of your CrazyGames preset:

    ```js
    <div id="banner-container" style="width: 300px; height: 250px; position: fixed; top:0; left:0; z-index:9999"></div>
    ```

    Within the singleton managing your SDK, request a banner like this:

    ```gd
    func requestBanner(width : int, height : int):
        var params = JavaScript.create_object("Object")
        params["id"] = "banner-container"
        params["width"] = width
        params["height"] = height
        SDK.banner.requestBanner(params)
    ```

    The id has to be the one you used in the head include, and the banner has to fit inside the container.

=== "Cocos"

    🟥 Not supported

## Request responsive banner

The responsive banners feature will request ads that fit into your container, without the need to specify or select a size beforehand. The resulting banners will have one of the following sizes:

-   970x90
-   320x50
-   160x600
-   336x280
-   728x90
-   300x600
-   468x60
-   970x250
-   300x250
-   250x250
-   120x600

Only banners that fit into your container will be displayed, if your container cannot fit any of these sizes no ad will be rendered. The rendered banner is automatically vertically and horizontally centered into your container.

=== "HTML5"

    Set your container size to a non-null value:
    ``` html
    <div id="responsive-banner-container" style="width: 500px; height: 500px"></div>
    ```
    Request the responsive banner:
    ```js
    try {
      // await is not mandatory when requesting banners, but it will allow you to catch errors
      await window.CrazyGames.SDK.banner.requestResponsiveBanner("responsive-banner-container");
    } catch (e) {
      console.log("Error on request responsive banner", e);
    }
    ```

=== "Unity"

    Not available for Unity

=== "GameMaker"

    Similar to static banners, you can request responsive banners like this:

    ```gml
    crazy_banner_request_responsive_banner(
        0,
        160, 600,
        CRAZY_BANNER_POSITION.CENTER_MIDDLE,
        function() { show_debug_message("Responsive banner loaded!"); },
        function(err) { show_debug_message("Responsive banner failed: " + json_stringify(err)); }
    );
    ```

=== "Construct"

    Not available

=== "Godot"

    Define a container in the `Head Include` of your CrazyGames preset with a non-null size:

    ```js
    <div id="responsive-banner-container" style="width: 500px; height: 500px; position: fixed; top:0; left:0; z-index:9999"></div>
    ```

    Make sure to position the container in a way so it does not cover gameplay or interface. Now request a responsive banner in script:

    ```gd
    SDK.banner.requestResponsiveBanner("responsive-banner-container")
    ```

=== "Cocos"

    🟥 Not supported

## Refreshing & clearing banners

=== "HTML5"

    To refresh the banners, simply call the `requestBanner` or `requestResponsiveBanner` methods again with the same container id.

    The banners have the following limitations:

    - There is a minimum delay of 60 seconds between banner refreshes. If you call the request banner methods more often, you will receive the following error: `A banner has already been requested for container banner-container less than 57 seconds ago, please wait.`
    - During a gaming session the banners can be refreshed up to 60 times (this applies to each banner size separately).

    **Clearing the banners**

    The SDK provides 2 methods for clearing the banners:

    ```js
    window.CrazyGames.SDK.banner.clearBanner("banner-container");
    // or
    window.CrazyGames.SDK.banner.clearAllBanners();
    ```

    We recommend that you clear the banners after hiding them. Otherwise, when you request new banners again, the old banners may still appear for a fraction of a second, which negatively impacts the user experience.

=== "Unity"

    Adding and positioning the banners in your scene is only part of what takes to display banners. Since the banners are rendered with JavaScript above your game, you also need to manually request a banner refresh.

    This is done by calling the following method:

    ```cs
    CrazySDK.Banner.RefreshBanners()
    ```

    The method needs to be called:

    -   when you want to refresh the banners
    -   when you load a scene that has some visible banners from the start
    -   after showing/hiding banners, for example when transitioning between different menus
    -   after navigating to another scene, so the banners displayed in the previous scene are cleared. For a better user experience, before leaving a scene that contains banners, disable them by calling `SetActive(false)`, and call the `CrazySDK.Banner.RefreshBanners()` method.

=== "GameMaker"

    This will clear all banners:

    ```gml
    crazy_banner_clear_all_banners()
    ```

    To clear a single banner, use these functions, passing the `banner_id`:

    ```gml
    crazy_banner_clear_banner(0)
    crazy_banner_clear_responsive_banner(0)
    ```

=== "Construct"

    If you need to hide all the displayed banners, call this method:

    ```js
    await window.ConstructCrazySDK.banner.hideAllBanners();
    ```

=== "Godot"

    To refresh the banners, simply call again the `requestBanner` or `requestResponsiveBanner` methods.

    The SDK provides 2 methods for clearing the banners:

    ```gd
    SDK.banner.clearBanner("banner-container")
    # or
    SDK.banner.clearAllBanners()
    ```

    We recommend that you clear the banners after hiding them. Otherwise, when you request new banners again, the old banners may still appear for a fraction of a second, which negatively impacts the user experience.

=== "Cocos"

    🟥 Not supported

## Limitations

Banners won't display if they do not follow any of these rules:

-   You can display a maximum of 2 banners of the same size at the same time.
-   The same banner can be re-displayed only 60 seconds after the last display.
-   The banner has to be fully inside the game window.
