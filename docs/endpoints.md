# API Endpoints

This document provides a detailed list and explanation of the API endpoints exposed by the LyricsPlus Backend. These endpoints allow for fetching lyrics, metadata, and submitting user contributions.

## General Structure

All primary API endpoints are organized modularly within the `src/modules/` directory. Each module's handler defines its specific routes.

## Available Endpoints

### Lyrics Retrieval

*   **`GET /v1/lyrics/get`**
    *   **Description**: Fetches synchronized lyrics for a given song in a legacy format, primarily for older YouLy+ clients.
    *   **Parameters**:
        *   `title` (required): The title of the song.
        *   `artist` (required): The artist of the song.
        *   `album` (optional): The album of the song.
        *   `duration` (optional): The duration of the song in milliseconds.
        *   `isrc` (optional): The ISRC of the song.
        *   `platformId` (optional): The platform-specific ID of the song. (appleMusicId/musixmatchTrackId,SpotifySongId)
        *   `source` (optional): Comma-separated list of preferred lyric sources (e.g., `musixmatch,spotify`).
        *   `forceReload` (optional): Set to `true` to bypass cache and force a reload of lyrics.
    *   **Response**: Returns lyrics in a legacy format (v1).

*   **`GET /v2/lyrics/get`**
    *   **Description**: Fetches synchronized lyrics for a given song in an improved format with better word-grouping.
    *   **Parameters**: (Same as `/v1/lyrics/get`)
    *   **Response**: Returns lyrics in the default format (v2).

*   **`GET /v1/ttml/get`**
    *   **Description**: Retrieves lyrics specifically formatted in TTML (Timed Text Markup Language), often used for Apple Music applications.
    *   **Parameters**: (Same as `/v1/lyrics/get`)
    *   **Response**: Returns lyrics content in Apple's TTML format.

### Song Catalog

*   **`GET /v1/songlist/search`**
    *   **Description**: Searches the song catalog for songs matching a given query.
    *   **Parameters**:
        *   `q` (required): The search query (e.g., song title, artist name).
    *   **Response**: Returns a list of songs matching the query.

### Metadata Retrieval

*   **`GET /v1/metadata/get`**
    *   **Description**: Obtains comprehensive metadata for a song, primarily from Apple Music.
    *   **Parameters**:
        *   `title` (required): The title of the song.
        *   `artist` (required): The artist of the song.
        *   `album` (optional): The album of the song.
        *   `duration` (optional): The duration of the song in milliseconds.
    *   **Response**: Returns song details such as title, artist, album, cover art URL, release date, etc.

### User Submissions

*   **`GET /v1/lyricsplus/challenge`**
    *   **Description**: Initiates a Proof-of-Work (PoW) challenge for user submission. Clients should request this endpoint to get a challenge token and difficulty before submitting lyrics.
    *   **Parameters**: None.
    *   **Response**: Returns a JSON object containing `token` (a JWT challenge) and `difficulty` (the required PoW difficulty).

*   **`POST /v1/lyricsplus/submit`**
    *   **Description**: Allows users to submit their own synchronized lyrics to the LyricsPlus database after successfully completing a Proof-of-Work challenge.
    *   **Request Body**:
        *   `proofOfWorkToken` (required): The JWT token obtained from `/v1/lyricsplus/challenge`.
        *   `nonce` (required): The solution to the Proof-of-Work challenge.
        *   `songTitle` (required): The title of the song.
        *   `songArtist` (required): The artist of the song.
        *   `songAlbum` (optional): The album of the song.
        *   `songDuration` (required): The duration of the song in milliseconds.
        *   `lyricsData` (required): The synchronized lyrics content (e.g., in LRC or KPOE format).
        *   `forceUpload` (optional): Boolean. Set to `true` to force upload, potentially bypassing some existing data checks.
    *   **Response**: Confirmation of submission or error details.

### Test Endpoints

*   **`GET /v1/test/musixmatch`**
    *   **Description**: A non-functional placeholder endpoint for Musixmatch integration testing. This endpoint does not currently provide any active functionality.
    *   **Parameters**: None.
    *   **Response**: Empty or placeholder response.

## Error Handling

All endpoints will return standard HTTP status codes for success and error conditions. Error responses will typically include a JSON object with an `error` message and potentially an `errorCode`.

*   `200 OK`: Request successful.
*   `400 Bad Request`: Invalid parameters or request body.
*   `404 Not Found`: Resource (e.g., song, lyrics) not found.
*   `500 Internal Server Error`: Server-side error.

---

*Note: This document provides a high-level overview. For exact parameter names, types, and detailed response schemas, please refer to the API specification or the source code within `src/modules/`.*
