# komikku-config-backup
This guide provides a comprehensive framework for installing, configuring, and restoring [Komikku](https://github.com/komikku-app/komikku). Following these steps ensures a high-performance reading environment with optimized metadata, tracking integration, and custom visual assets.

## Table of Contents

- [1. Appropriate Release](#1-appropriate-release)
  - [APK Selection Guide](#apk-selection-guide)

- [2. Environment Setup](#2-environment-setup)
  - [A. Interface Personalization](#a-interface-personalization)
  - [B. Storage Configuration](#b-storage-configuration)
  - [C. Grant System Permissions](#c-grant-system-permissions)

- [3. Extension Repo Configuration](#3-extension-repo-configuration)
  - [A. Accessing Extensions](#a-accessing-extensions)
  - [B. Adding Extension Repo](#b-adding-extension-repo)

- [4. Required Extensions](#4-required-extensions)
  - [A. Installation Procedure](#a-installation-procedure)
  - [B. Required Extensions List](#b-required-extensions-list)

- [5. Extension Trust Management](#5-extension-trust-management)
  - [A. Identifying Untrusted Extensions](#a-identifying-untrusted-extensions)
  - [B. How to Trust an Extension](#b-how-to-trust-an-extension)
  - [C. Why This Even Matters](#c-why-this-even-matters)

- [6. Expanded Extension List](#6-expanded-extension-list)

- [7. Source Verification and Tracker](#7-source-verification-and-tracker)
  - [A. Verify Installed Sources](#a-verify-installed-sources)
  - [B. Configure Tracking Services](#b-configure-tracking-services)

- [8. Data Restoration and Deployment](#8-data-restoration-and-deployment)
  - [A. Download Required Assets](#a-download-required-assets)
  - [B. Configure Cover Directory](#b-configure-cover-directory)

- [9. Execute Backup Restoration](#9-execute-backup-restoration)

- [10. Final Verification and Library Check](#10-final-verification-and-library-statistics)

## 1. Appropriate Release

| **Type**        | **Link** |
|-------------------------|:---------:|
| **Beta Release**  | [![Komikku Preview](https://img.shields.io/github/v/release/komikku-app/komikku-preview.svg?maxAge=3600&label=Komikku-Preview&labelColor=2c2c47&color=1c1c39)](https://github.com/komikku-app/komikku-preview/releases/latest) |
| **Stable Release**| [![Komikku Legacy](https://img.shields.io/github/release/komikku-app/komikku.svg?maxAge=3600&label=Komikku-Stable&labelColor=06599d&color=043b69)](https://github.com/komikku-app/komikku/releases/latest) |

Scroll down and Choose an `.apk` file. For optimal compatibility with the latest extension APIs and user interface enhancements, it is strongly **recommended to install the Lastest Beta/Preview release** of Komikku. The stable version frequently lags behind critical source updates and may not support the newest features.

The Beta version incorporates essential fixes for extension scrapers and improved database management. Always select the `.apk` file explicitly labeled as `Beta` to ensure full functionality. You can also download Komikku on their official site's [Download](https://komikku-app.github.io/download/) page by clicking the grayish preview button on the right side of that page.

### `.apk` Selection Guide
| Type        | Architecture              | Device                          | Advantage                                      | Limitation                                      | Usage                                      |
|-----------------|--------------------------|------------------------------------------|------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| `arm64-v8a`     | 64-bit ARM              | Modern devices (post-2015, most phones)  | Best performance, smaller size                 | None significant                                 | Always use if device supports 64-bit             |
| `armeabi-v7a`   | 32-bit ARM               | Older / low-end devices                  | Legacy compatibility                           | Less efficient, may require larger APKs          | Only if 64-bit is not supported                  |
| `universal`     | 32-bit + 64-bit (multi)  | Unknown or mixed device environments     | Works on almost all devices, easy distribution | Larger size, slightly higher memory usage        | Use when unsure of device or sharing widely      |

> [!Tip]
> Download/Install `arm64-v8a.apk` version

## 2. Environment Setup
Before using Komikku effectively, it is important to configure the application’s environment and interface to ensure optimal usability and aesthetic consistency.

### A. Interface Personalization
On the first launch (or after a fresh installation), Komikku will prompt you to select a **Mode** and a **Theme**. For the best visual experience:
* Select `Dark Mode`: reduces eye strain and provides a clean, modern interface.
* Enable `Dynamic Theme`: this feature automatically adapts primary color accents based on the currently displayed manga cover, ensuring a cohesive and immersive visual experience. The dynamic theme enhances readability and keeps the interface visually aligned with your manga content (cover).

### B. Storage Configuration
After selecting the theme, proceed by clicking **Next**. Komikku will then prompt you to choose a directory for storing application data. This directory will serve as the application's **Home folder**, where all downloads, cached files, and local library data are managed.
1. Click **"Select a Folder"** button. This will open your device’s file manager / main file manager.
2. Navigate to the **root directory** of your internal storage. (To do this and) If you are currently inside another folder, move back to the main storage view. You can just press the back button, or open the **menu ☰ in the top-left corner** and select your **device name**.
    - You will know you are in the correct location when you see common folders such as: `Android`, `DCIM`,  `Documents`,  `Download` etc.
3. Create a new folder beside them: Tap the **"New Folder"** or **+ icon**, usually located at the top-right. Name the folder exactly as `Komikku` 
4. Open this newly created **"Komikku"** folder (if it does not open automatically).
5. Tap **"Use this folder"** button located below, then grant the required permissions by selecting **Allow** when prompted.
6. You will be redirected back to Komikku, where the selected folder's path will now be displayed above the **"Select a Folder"** button (don't click on it again). Typical directory path should look like this:
    ```
    /storage/emulated/0/Komikku
    ```
7. Click **Next** to continue.

### C. Grant System Permissions
To ensure seamless operation particularly for background synchronization, extension updates, and download management etc; you must **Grant** the required system permissions during setup. Enable the following permissions by clicking the **"Grant"** button next to each option:
- **Install Unknown Apps**: Allows Komikku to install and update extensions internally without requiring manual APK installation.
- **Notifications**: Enables alerts for library updates, download completion, and important system messages.
- **Background Battery Usage**: Set this to **"Don’t Optimize"** or **"Unrestricted"**. This prevents Android from suspending Komikku in the background, which is critical for: Automatic tracking updates, Background downloads, Sync operations.
- **Access to Media Files**: Grants permission to read and write storage data. This is required for: Saving downloaded chapters, Displaying local content, Managing cached images and files.
- Click **Next** to continue
- On the next page Click the **Get Started** button located below. (We still have some work to do before restoring our backup file) So we will be skipping this Stage by Clicking **Get started**.

> [!Note]
> Many modern Android systems (especially from brands like Xiaomi, Samsung, or Realme) aggressively restrict background activity to save battery. Even after granting permissions, you may need to manually disable battery optimization for Komikku in your device settings to ensure uninterrupted performance. It depends on your Device and Android version you're using.

## 3. Extension Repo Configuration
Komikku relies on external repositories to fetch and manage source extensions. Proper repository configuration is essential to avoid duplication, reduce conflicts, and ensure long-term stability.

### A. Accessing Extensions
1. Navigate to your **Library** in the Komikku app (after clicking **Get Started** you will already be there, and it will be empty at this stage).
2. Tap the **Browse** icon in the bottom navigation bar.
3. Select the **Extensions** tab at the top bar (third tab from the left on the top bar).

### B. Adding Extension Repo
1. Tap the **three-dot menu ⋮** in the top-right corner.
2. Select **Extension Repositories** to open the repository management page.
3. Adding the **Keiyoushi repository**:
    - Tap the **Add (+)** button (ar the bottom-right corner).
    - Paste the following URL in the prompted text field:
        ```
        https://raw.githubusercontent.com/keiyoushi/extensions/repo/index.min.json
        ```
    - Tap **Add**. The repository will load along with its icon and available controls.

4. Adding the **Yūzōnō repository**:
    - Tap the **Add (+)** button again.
    - Paste the following URL in the prompted text field:
        ```
        https://raw.githubusercontent.com/yuzono/manga-repo/repo/index.min.json
        ```
    - Tap **Add**. The repository will load along with its icon and available controls.

5. At this point disable the **Keiyoushi repository** by toggling the **Eye icon** within its section. It should be treated strictly as a **fallback source**.

    > The **Yūzōnō repository** already includes all Keiyoushi extensions, along with additional exclusive sources. Keeping both active may result in duplicate entries and potential conflicts.

    > If at some point in time, the Yūzōnō repository becomes unavailable you can re-enable Keiyoushi at any time by toggling the Eye icon again.

6. Return back to the **Extensions** tab and **Swipe down to refresh** the page. Once refreshed, the application will populate with a comprehensive list of available extensions, ready for installation.

> [!Note]
> If you want to install extension manually or want to find more info on the extension or their repo then visit [Wotaku](https://wotaku.wiki/guides/ext/mihon). You can even follow their guide to easily add the repo in your Komikku with just one or two clicks.

## 4. Required Extensions
To ensure a successful restoration of the provided backup file, certain core extensions must be installed in advance. If these extensions are missing, Komikku will not be able to map existing library entries correctly, resulting in "Missing Source" errors during the restore process.

### A. Installation Procedure
1. Navigate to **Browse** → **Extensions** if you're not already there; there should be hundreds of extensions ready to be installed.
2. Use the search function (the search icon on top) or scroll through the list to locate the required extensions.
3. Tap the **Install icon** next to each extension.
4. Wait for all installations (of the required extensions) to complete before proceeding. You can scan or skip the installation if your android suggest to scan.

### B. Required Extensions List

| Source        | Language | Entries | Version | Provider | 18+ |
| :------------ | -------- | ------- | ------- | -------- | --- |
| Asura Scans   | English  | 2       | 1.4.58  | Yūzōnō   | No  |
| Comix         | English  | 3       | 1.4.8   | Yūzōnō   | Yes |
| Gourmet Scans | English  | 1       | 1.4.46  | Yūzōnō   | Yes |
| MangaBuddy    | English  | 1       | 1.4.22  | Yūzōnō   | Yes |
| MangaDex      | English  | 200     | 1.4.206 | Yūzōnō   | Yes |
| MangaFire     | Japanese | 1       | 1.4.19  | Yūzōnō   | Yes |
| MangaFire     | English  | 34      | 1.4.19  | Yūzōnō   | Yes |
| MangaForest   | English  | 6       | 1.4.21  | Yūzōnō   | Yes |
| MangaFox      | English  | 1       | 1.4.7   | Yūzōnō   | Yes |
| Manganato     | English  | 1       | 1.4.15  | Yūzōnō   | Yes |
| Webtoons.com  | English  | 4       | 1.4.52  | Yūzōnō   | No  |
| Weeb Central  | English  | 210     | 1.4.18  | Yūzōnō   | Yes |

> [!Important]
> Search for and install all the extensions listed above without selecting alternative variants or duplicates.

> Extension versions and NSFW (18+) labels may change over time. These differences are expected and do not affect compatibility with the backup.

> The table above also indicates the number of entries sourced from each extension within the backup file. You can ignore that colum  as it won't be shown at this stage, This just provides a reference to verify that all necessary sources are properly installed and has the extact number of entries **after going through and finishing this whole guide**, just incase you need to varify your setup is alright or not.

## 5. Extension Trust Management
Komikku includes a **trust verification system** for extensions, similar to other apps in the Tachiyomi ecosystem. Extensions may be marked as untrusted, especially after reinstalling the app or restoring a backup.

### A. Identifying Untrusted Extensions

- A red `untrusted` label below some extension name. If you already had them (installed extensions) before installing Komikku, i.e. in the case of reinstalling Komikku or migrating from [Mihon](https://mihon.app/) or any Tachiyomi successor.

  > Because when you uninstall Komikku, the Komikku app gets uninstalled for sure but not the extension you downloaded inside or through it. Meaning, the extensions you installed are still in your device even after uninstalling Komikku (if you want to remove them you have to uninstall them one by one from your App Management section in your Device Settings). Thus when migrating from Mihon or reinstalling Komikku you will find your existing extensions in the **Extension** tab already present even before you add or load the extension repo. As they weren't installed through Komikku and were already present before installing Komikku, Komikku will need to know if it's safe to use them.

### B. How to Trust an Extension
1. An **× icon** on the left side of the extension, pressing on it would indicate the extension is not safe so Komikku won't use it.
2. A **shield icon** on the right side of the extension, pressing on it would indicate it's safe to use this extension.
3. Tap the **shield icon** next to the extension's right side.
4. Select **"Trust this extension"**, Once trusted all warning indicators will disappear and the extension will function normally.

### C. Why This Even Matters
Untrusted extensions cannot: Load content, Fetch updates, Download chapters. Granting trust allows the extension to fully interact with Komikku’s system.

> [!Note]
> For people freshly installing Komikku and who have never used any extensions or Tachiyomi related successors then you won't see trust management issue above at all.

## 6. Expanded Extension List
Here's the extended version of the above required extension list, containing all the extension that might be useful for you. You may install this too for a better experience, it's up to you. Each of them take less than 2 MB of space, maybe even less depending on their version or time.

| Source          | Language | Version | 18+ |
| :-------------- | -------- | ------- | --: |
| Aein Scans      | English  | 1.4.19  |  No |
| Asura Scans     | English  | 1.4.58  |  No |
| Athrea Scans    | English  | 1.4.30  | Yes |
| BoxManhwa       | English  | 1.4.20  | Yes |
| Comix           | English  | 1.4.8   | Yes |
| Gourmet Scans   | English  | 1.4.46  | Yes |
| Hades Scans     | English  | 1.4.30  |  No |
| InfinityScans   | English  | 1.4.7   | Yes |
| Lua Scans       | English  | 1.4.49  |  No |
| Lunar Manga     | Multi    | 1.4.3   | Yes |
| Manga Ball      | Multi    | 1.4.2   | Yes |
| Manga Planet    | English  | 1.4.2   | Yes |
| Mangabat        | English  | 1.4.17  | Yes |
| MangaBTT        | English  | 1.4.3   | Yes |
| MangaBuddy      | English  | 1.4.22  | Yes |
| MangaCrazy      | Multi    | 1.4.46  | Yes |
| MangaDex        | Multi    | 1.4.206 | Yes |
| MangaDia        | English  | 1.4.46  |  No |
| MangaFab        | English  | 1.4.20  | Yes |
| MangaFire       | Multi    | 1.4.19  | Yes |
| MangaForest     | English  | 1.4.21  | Yes |
| MangaFox        | English  | 1.4.7   | Yes |
| Manganato       | English  | 1.4.15  | Yes |
| MangaNow        | English  | 1.4.3   | Yes |
| Manhwa Comics   | English  | 1.4.46  | Yes |
| Manhwa-raw      | Multi    | 1.4.48  | Yes |
| ManhwaBuddy     | English  | 1.4.2   | Yes |
| ManhwaClub. Net | Multi    | 1.4.46  | Yes |
| ManhwaHub       | English  | 1.4.4   | Yes |
| Manhwajoy       | English  | 1.4.46  | Yes |
| ManhwaRead      | English  | 1.4.1   | Yes |
| ManhwaZ         | English  | 1.4.39  | Yes |
| Omega Scans     | English  | 1.4.47  | Yes |
| Qi Scans        | English  | 1.4.22  |  No |
| The Blank       | English  | 1.4.53  | Yes |
| Vortex Scans    | English  | 1.4.55  |  No |
| Webtoons. Com   | Multi    | 1.4.52  |  No |
| WebtoonScan     | English  | 1.4.46  | Yes |
| WebtoonXYZ      | English  | 1.4.50  | Yes |
| Weeb Central    | English  | 1.4.18  | Yes |

## 7. Source Verification and Tracker
After installing all required extensions, perform the following configuration steps:

### A. Verify Installed Sources
1. Navigate to **Browse** from the buttom nav inside Komikku, Go to **Sources** tab (the first tab from the left at the top bar)
2. Confirm that all previously installed extensions are listed and accessible. (you may click one extension and see if it loads or fetches its contents). This ensures that Komikku has successfully recognized and loaded the extensions.

### B. Configure Tracking Services
1. Open the **More** menu located in the bottom-right corner.
2. Go to **Settings** then to **Tracking**.
3. Connect your tracking accounts:
	- Firstly keep all the options toggled on. Don't turn off anything.
    - Select [MyAnimeList](https://myanimelist.net/).
    - You will be redirected to a browser or in-app WebView.
    - If you're already logged in then just simply grant permission. If you're not logged in not then log in to your myanimelist account first, then return in the **Tracking** page again in Komikku and repeat the authorization step by clicking the **MyAnimeList** again.
    - Repeat the same process for [AniList](https://anilist.co/)
  
4. Once successfully connected: A **green checkmark** will appear next to each service. This indicates that the tracking integration is active and functioning.

## 8. Data Restoration and Deployment
This step restores the complete library along with pre-configured settings and high-quality custom covers. The provided backup is designed to deliver a fully configured experience with minimal manual setup.

### A. Download Required Assets
1. **Backup Database**: Download the `.tachibk` backup file from the provided google drive source [Click Here](https://drive.google.com/file/d/129RYG4KDNR4FflRM0mzQ5MgGJyGvgCYN/view?usp=drivesdk). You may need to use the desktop view mode inside your browser to be able to download it.

2. **Custom Cover Archive**: Download the covers folder named as **"komikku_custom_covers1"** from the provided google drive source [Click Here](https://drive.google.com/drive/folders/1PkgKPPUxJDFQw42hZW1eOpf8Uw2taEQh). You may need to use the desktop view mode inside your browser to be able to download it.
3. The folder will be automatically compressed into a `.zip` file during download (google drive does that automatically).
4. Extract the archive using your file manager or tools such as ZArchiver.

> [!Important]
> Ensure the archive is extracted as a **complete folder**. Do not extract files individually or directly without a directory structure or folder, as this may break cover mapping during restoration. In short, After unzipping the folder you should see a folder named "**komikku_custom_covers1**" at wherever you unzipped it.

### B. Configure Cover Directory
For the backup to correctly associate covers with library entries, files must be placed in a specific directory or folder beforehand:

1. Navigate to your **Internal Storage** then to **Pictures** folder the path of which should look exactly like this:
    ```
    /storage/emulated/0/Pictures/
    ```

4. Ensure a folder named `Komikku` exists here too (if not then create a folder exactly named as `Komikku`). Note: This Komikku folder is **inside the Pictures directory** and is different from the Komikku folder we created earlier in the Internal Storage directly, whose path looked like:
   ```
   /storage/emulated/0/Komikku
   ```
    
5. Open the extracted **komikku_custom_covers1** and Copy all the contents, images, accompanying `.txt` files etc. Remember the file count in that folder is 477 files.
7. Paste all those 477 copied files into the Komikku folder that is inside the Picture folder, again whose path looked like this:
   ```
   /storage/emulated/0/Pictures/Komikku/
   ```

> [!Note]
> Placing files in the wrong location or changing any file's name or folder path will result in missing or incorrectly assigned covers. Proper placement of these assets ensures that all library entries are restored with their intended visual metadata, maintaining consistency with the original configuration.

## 9. Execute Backup Restoration
- Open **Komikku**
- Navigate **Settings** then to **Data and Storage** click on the **Restore Backup** button.
- Locate and select the downloaded `.tachibk` backup file (that single file you got from my google drive)
- Press **Okay** or **Done** (if prompted).
- Wait for the process to complete.
- You can monitor progress via the notification panel.

> [!Note]
It is normal to encounter minor errors or mostly found "2 Errors" after restoration in this case. These typically result from duplicate repository entries already configured earlier and do not affect functionality. You can safely ignore them.

## 10. Final Verification and Library Statistics
After restoring your Komikku backup, it is essential to perform a thorough verification to ensure the integrity of your library, metadata, and tracking functionality. Follow the steps below:

### Verification Checklist
1. **Library**: Confirm that all titles appear correctly with their respective metadata, including authors, genres, and chapters.

2. **Covers**: Ensure high-resolution custom covers load immediately. If covers fail to load, recheck the directory path configured in **Section 8, B**.

3. **Updates**: Navigate to the **Updates** section via the bottom navigation bar. Swipe down to refresh the library, fetching the latest chapters for all entries.

4. **Tracking**:
    - Open any manga or title and click the **Tracking** button. Select either **MyAnimeList (MAL)** or **AniList**. You may also select both trackers simultaneously, it's up to you (I personally add both tracker for each entry as I use both of them trackers). The app will search for the title in the chosen database and allow you to select the correct entry.
    - Once connected, you can update the following:
        - Reading status
        - Chapters read
        - Personal rating
        - Start and finish dates.
    - **Two-way synchronization** is supported: marking a chapter as unread or read in Komikku will automatically update MAL or AniList accordingly.
5. **Sources**: Browse a few titles to confirm that the **Source** tab loads chapters correctly. Test the search functionality in the **Browse** section to verify all sources are operational.

### Library Statistics
#### As of Backup Creation

| Category                    | Count   |
| --------------------------- | ------- |
| Total Count of Entries      | 465     |
| Backup `.tachibk` file size | 2.2 MB  |
| Cover Folder Size           | 860 MB  |
| App Disk Usage              | 900 MB  |
| Total Disk Usage            | ~1.8 GB |

### **Categorization Breakdown**

| Category | Count | Type / Priority     |
| -------- | ----- | ------------------- |
| `MAS1`     | 206   | S-Class Manga       |
| `MAS2`     | 152   | S-Class Manga       |
| `MAN1`     | 46    | Normal Manga        |
| `MAN2`     | 20    | Normal Manga        |
| `RDN1`     | 0     | Read Priority: High |
| `RDN2`     | 0     | Read Priority: Low  |
| `MWS1`     | 22    | S-Class Manhwa      |
| `MWS2`     | 8     | S-Class Manhwa      |
| `MWN1`     | 4     | Normal Manhwa       |
| `MWN2`     | 8     | Romance Manhwa      |

I typically place manga I plan to read this week in `RDN1` and manga planned for the month or later in `RDN2`.

> Categories can be customized if you go to **More ⋮** then go to **Categories**. It allows you to create, hide, or rearrange categories.

> To assign a manga to a category, open the entry and click the heart icon. By default, new manga are added to `RDN1`, but holding the heart icon lets you select one or multiple categories.

## Conclusion
Following these steps ensures that your Komikku library is fully restored, properly categorized, and synchronized with tracking services. Verifying the library, covers, updates, and sources prevents potential data inconsistencies, while the category system allows for a personalized reading workflow. By completing this verification, you can confidently continue enjoying your manga and manhwa collection with seamless progress tracking and efficient library management.
