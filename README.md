# GIPHY Gallery

An Android application that loads trending GIFs from the GIPHY API and displays them in an infinite Pinterest-style feed.

The project was created as part of **Homework #2 — Loading Data from an API and Handling UI States**.

## Features

- Loads trending GIFs from the GIPHY API
- Displays animated GIFs
- Pinterest-style staggered grid with different image proportions
- Infinite scrolling with automatic pagination
- Initial loading indicator
- Pagination loading indicator at the bottom of the feed
- Initial loading error state with a retry button
- Pagination error handling without hiding already loaded content
- Data preservation across configuration changes using `ViewModel`
- Dynamic image aspect-ratio calculation
- In-memory caching of API responses
- Memory and disk image caching with Coil
- Full-screen GIF preview on item tap
- API key stored outside the source code

## Tech Stack

- Kotlin
- Jetpack Compose
- Material 3
- ViewModel
- StateFlow
- Kotlin Coroutines
- Retrofit 2
- Gson
- OkHttp
- Coil
- Coil GIF
- GIPHY API

## Architecture

The project uses a simple MVVM-style structure:

```text
Jetpack Compose UI
        |
        v
GiphyViewModel
        |
        v
GiphyRepository
        |
        v
Retrofit / GIPHY API
```

### UI Layer

The UI is built with Jetpack Compose.

`GiphyScreen` observes state from `GiphyViewModel` and displays the appropriate screen depending on the current state:

- initial loading;
- loaded content;
- initial loading error;
- pagination loading;
- pagination error.

GIFs are displayed with `LazyVerticalStaggeredGrid`, which creates a Pinterest-style layout with items of different heights.

### ViewModel

`GiphyViewModel` manages:

- the loaded GIF list;
- initial loading state;
- pagination loading state;
- initial loading errors;
- pagination errors;
- pagination offset;
- information about whether more content is available.

The state is stored in a `ViewModel`, so loaded content survives configuration changes such as screen rotation.

### Data Layer

`GiphyRepository` communicates with the GIPHY API through Retrofit.

The repository supports both trending GIF requests and search requests. API responses are cached in memory for a limited time to reduce unnecessary network calls.

## Pagination

The feed uses infinite scrolling.

When the user approaches the end of the loaded list, the application automatically requests the next page.

While the next page is loading, a progress indicator is displayed at the bottom of the grid. Existing content remains visible.

If pagination fails, a retry button is shown at the bottom of the feed.

## Image Loading and Caching

Images are loaded with Coil using a custom `ImageLoader`.

The application enables:

- memory caching;
- disk caching;
- GIF decoding;
- caching for animated images.

GIF dimensions are read to calculate the image aspect ratio dynamically, allowing cards with different proportions to be displayed correctly in the staggered grid.

## GIF Preview

Tapping a GIF opens it in a full-screen dialog.

The GIF is displayed with its original proportions using `ContentScale.Fit` and can be closed with the button in the top-right corner.

## Project Structure

```text
app/src/main/
├── java/com/example/apiapp/
│   ├── MainActivity.kt
│   ├── GiphyScreen.kt
│   ├── GiphyViewModel.kt
│   ├── GiphyRepository.kt
│   ├── GiphyApiService.kt
│   ├── GiphyResponse.kt
│   ├── RetrofirInstance.kt
│   ├── ImageCasheManager.kt
│   ├── PinterestGrid.kt
│   ├── PinterestCard.kt
│   ├── DetailScreen.kt
│   ├── ErrorScreen.kt
│   ├── LoadingIndicator.kt
│   └── RetryButton.kt
│
└── res/
    ├── drawable/
    ├── mipmap-*/
    ├── values/
    │   ├── colors.xml
    │   ├── dimens.xml
    │   ├── integers.xml
    │   ├── strings.xml
    │   └── themes.xml
    └── xml/
```

## Getting Started

### Requirements

- Android Studio
- Android SDK
- JDK 11 or newer
- Minimum Android SDK: 24
- GIPHY API key

### 1. Clone the repository

```bash
git clone https://github.com/kaneli-pulla/homework2_android_vk.git
cd homework2_android_vk
```

### 2. Create a GIPHY API key

Create an application in the [GIPHY Developers](https://developers.giphy.com/) dashboard and obtain an API key.

### 3. Configure the API key

Add the following line to the project's `local.properties` file:

```properties
GIPHY_API_KEY=YOUR_API_KEY
```

Example:

```properties
sdk.dir=/path/to/Android/sdk
GIPHY_API_KEY=your_giphy_api_key_here
```

The API key is read into `BuildConfig` during the build and does not need to be stored directly in the source code.

### 4. Run the application

1. Open the project in Android Studio.
2. Wait for Gradle synchronization to finish.
3. Select an emulator or connect a physical Android device.
4. Run the `app` configuration.

You can also build a debug APK from the command line:

```bash
./gradlew assembleDebug
```

On Windows:

```bash
gradlew.bat assembleDebug
```

## API

The application uses the GIPHY API.

Base URL:

```text
https://api.giphy.com/v1/
```

The main endpoint used by the feed is:

```text
GET /gifs/trending
```

Pagination is implemented with the `limit` and `offset` query parameters.

The data layer also contains support for:

```text
GET /gifs/search
```

## Homework

This project was developed for Homework #2, focused on loading data from an API and correctly handling asynchronous UI states.

The implementation includes loading states, pagination, retry behavior, caching, configuration-change state preservation, animated GIF support, dynamic image proportions, and a Pinterest-style feed.

## Author

[kaneli-pulla](https://github.com/kaneli-pulla)
