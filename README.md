這是一個剛開發的 Astro 博客,模板來自 Astro 自帶模板,部署于 Vercel

# XPPlayer.astro

這是一個從 11ty shortcode `{% xpPlayer %}` 遷移出來的 Astro 單檔音樂播放器組件。

放置位置建議：

src/components/XPPlayer.astro

基本使用：

---

## import XPPlayer from "../components/XPPlayer.astro";

<XPPlayer
	src="[AUDIO_URL]"
	title="[SONG_TITLE]"
	artist="[ARTIST_NAME]"
	cover="[COVER_URL]"
	lrc="[LYRICS_URL]"
/>

Props:

- src: 音樂文件 URL。必填。不提供時不渲染播放器。
- title: 歌曲標題。選填。
- artist: 藝術家名稱。選填。
- cover: 封面圖片 URL。選填；不提供時會使用 fallbackCover，並嘗試從 MP3 ID3 標籤讀取封面。
- lrc: LRC 歌詞文件 URL。選填；提供後會顯示歌詞切換按鈕。
- fallbackCover: 預設封面 URL。選填，預設為 `/media/audio/no-album-art.png`。

功能：

- 播放 / 暫停
- 點擊進度條跳轉
- 顯示目前時間與總時長
- 同頁多個播放器時，自動暫停其他正在播放的播放器
- LRC 歌詞載入、同步高亮、點擊歌詞跳轉
- 沒有傳入 cover 時，透過 jsmediatags 嘗試讀取 MP3 ID3 封面
- 支援 Astro View Transitions 的 `astro:page-load` 重新初始化

注意事項：

- 若使用遠端音訊、封面或 LRC，目標服務需要允許瀏覽器跨域存取。
- ID3 自動封面功能會從 CDN 載入 `jsmediatags@3.9.7`。
- 若你不需要自動讀取 ID3 封面，可以移除 `loadJsMediaTags`、`bytesToBase64` 和 `ensureCoverArtwork` 裡的自動讀取邏輯。
- 目前樣式是 scoped 到此 Astro component；如果你拆成外部 CSS，請確認 `.xp-player` 相關類名仍可作用。

原 11ty 對應來源：

- `eleventy.config.js`: `xpPlayer` shortcode HTML
- `_includes/base.njk`: 播放器互動 JS
- `_includes/styles/index.css`: 播放器 CSS
- `_data/xpPlayer.js`: 預設文字、icon、fallbackCover 設定
