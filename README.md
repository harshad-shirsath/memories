<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Travel Gallery</title>

<style>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #ffffff;
    color: #222;
  }

  .page-title {
    text-align: center;
    font-family: Georgia, serif;
    font-size: 2.2rem;
    margin: 40px 0 20px;
  }

  /* ---------- Tabs bar ---------- */
  .tabs {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 22px;
    border-bottom: 1px solid #e5e5e5;
    padding-bottom: 14px;
    margin-bottom: 20px;
  }

  .tab {
    cursor: pointer;
    font-size: 0.95rem;
    color: #444;
    padding-bottom: 6px;
  }

  .tab.active {
    color: #000;
    font-weight: bold;
    border-bottom: 2px solid #000;
  }

  /* ---------- Trip description text (changes depending on the tab you click) ---------- */
  .trip-desc {
    max-width: 700px;
    margin: 0 auto 30px;
    padding: 0 20px;
    text-align: center;
    color: #555;
    font-size: 1rem;
    line-height: 1.6;
  }

  /* 
    ================= THE MASONRY GRID =================
    Instead of a rigid grid where every box is the same height,
    we use CSS "columns" — like a newspaper layout. Items flow down
    each column and naturally stack based on their own real height.
    This is what makes tall and short photos/videos fit together neatly,
    just like the Pinterest-style layout you wanted.
  */
  .gallery {
    column-count: 4;        /* how many columns to show on a wide screen */
    column-gap: 10px;
    padding: 0 20px 40px;
  }

  /* On smaller screens, use fewer columns so things don't get too squished */
  @media (max-width: 900px) {
    .gallery { column-count: 3; }
  }
  @media (max-width: 600px) {
    .gallery { column-count: 2; }
  }

  /* Every photo/video block */
  .item {
    position: relative;
    break-inside: avoid;   /* stops an item from splitting awkwardly across two columns */
    margin-bottom: 10px;
    border-radius: 6px;
    overflow: hidden;
    background: #eee;
    cursor: default;
  }

  /* 
    IMPORTANT: no fixed height and no object-fit:cover here anymore.
    "height: auto" means the image keeps its OWN natural shape —
    a tall portrait photo stays tall, a wide landscape photo stays wide.
  */
  .item img {
    width: 100%;
    height: auto;
    display: block;
  }

  /* Native video tags need the same sizing rule as images so they
     fit into the masonry columns the same way */
  .item video {
    width: 100%;
    height: auto;
    display: block;
  }

  /* Only video thumbnails get a pointer cursor, since only they're clickable */
  .item.video-item {
    cursor: pointer;
  }

  .item.hidden {
    display: none;
  }

  /* Add this class to any item that should stretch across the FULL width
     of the gallery instead of sitting inside just one narrow column.
     Used for the Goa Diary embed below. */
  .item.wide-item {
    column-span: all;
  }

  /* This hides the ENTIRE grid at once — used for the "All" tab, which
     shows only the description text with no photos or videos below it. */
  .gallery.hidden {
    display: none;
  }

  .duration-badge {
    position: absolute;
    top: 8px;
    left: 8px;
    background: rgba(0,0,0,0.65);
    color: #fff;
    font-size: 0.75rem;
    padding: 2px 8px;
    border-radius: 12px;
  }

  /* ---------- Popup video player ---------- */
  .lightbox {
    display: none;
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(0,0,0,0.85);
    z-index: 100;
    align-items: center;
    justify-content: center;
  }

  .lightbox.open { display: flex; }

  .lightbox-box {
    width: 90%;
    max-width: 800px;
    aspect-ratio: 16 / 9;
    position: relative;
  }

  .lightbox-box iframe {
    width: 100%;
    height: 100%;
    border: none;
    border-radius: 6px;
  }

  .close-btn {
    position: absolute;
    top: -40px;
    right: 0;
    color: #fff;
    font-size: 1.8rem;
    cursor: pointer;
  }
</style>
</head>
<body>

  <h1 class="page-title">My Travel Gallery</h1>

  <!-- 
    ================= TABS =================
    Same as before: each tab's data-tab must match the data-category
    on the matching photos/videos below.
  -->
  <div class="tabs" id="tabs">
    <div class="tab active" data-tab="all">All</div>
    <div class="tab" data-tab="korigad">Korigad Fort</div>
    <div class="tab" data-tab="lohagad">Lohagad Fort</div>
    <div class="tab" data-tab="manali">Manali Diary</div>
    <div class="tab" data-tab="goa">Goa Diary</div>
  </div>

  <!-- 
    ================= TRIP DESCRIPTION =================
    This text changes automatically depending on which tab is clicked.
    Write your own story for each trip inside the tripDescriptions
    list down in the JavaScript section near the bottom — search for
    "tripDescriptions" below to find where to edit these.
  -->
  <p class="trip-desc" id="tripDesc">
    A collection of photos and videos from everywhere I've traveled.
  </p>

  <!-- 
    ================= GALLERY (MASONRY GRID) =================
    - For a PHOTO: <div class="item"><img src="..."></div>
    - For a VIDEO: add class="video-item" and a data-video="VIDEO_ID" attribute.
    There's no text under individual photos/videos — the only description
    is the one paragraph above the grid, which changes per tab (edit it
    in the "tripDescriptions" section of the JavaScript below).
  -->
  <div class="gallery" id="gallery">

    <div class="item" data-category="korigad">
      <img src="https://www.playbook.com/s/memoriesio/ztmszaJPb9q6x8Ed2XPMDcBa?assetToken=sF2vvuCkU3VYLmhdfZbfGXSn">
    </div>

    <div class="item video-item" data-category="korigad" data-video="dQw4w9WgXcQ">
      <img src="https://img.youtube.com/vi/dQw4w9WgXcQ/hqdefault.jpg">
      <span class="duration-badge">▶ 0:17</span>
    </div>

    <div class="item" data-category="korigad">
      <img src="https://via.placeholder.com/400x300?text=Korigad+Photo+2">
    </div>

    <div class="item" data-category="lohagad">
      <img src="https://via.placeholder.com/400x700?text=Lohagad+Photo+1">
    </div>

    <div class="item video-item" data-category="lohagad" data-video="dQw4w9WgXcQ">
      <img src="https://img.youtube.com/vi/dQw4w9WgXcQ/hqdefault.jpg">
      <span class="duration-badge">▶ 0:34</span>
    </div>

    <div class="item" data-category="manali">
      <img src="https://via.placeholder.com/400x400?text=Manali+Photo+1">
    </div>

    <div class="item" data-category="manali">
      <img src="https://via.placeholder.com/400x600?text=Manali+Photo+2">
    </div>

    <div class="item wide-item" data-category="goa">
      <div style="height: 900px;">
        <iframe 
          src="https://playbook.com/e/memoriesio/muE9PF6eFpeTMhGoWtCG7Co8?theme=gallery&assetNumber=3&displaySize=large"
          title="Goa Diary - Playbook.com" 
          sandbox="allow-same-origin allow-scripts" 
          frameborder="0" 
          width="100%" 
          height="100%">
        </iframe>
      </div>
    </div>

  </div>

  <!-- Popup video player, hidden until a video is clicked -->
  <div class="lightbox" id="lightbox">
    <div class="lightbox-box">
      <span class="close-btn" id="closeBtn">&times;</span>
      <iframe id="lightboxFrame" src="" allowfullscreen></iframe>
    </div>
  </div>


  <script>

    // ================= WRITE YOUR OWN TRIP STORIES HERE =================
    // Each line matches a tab's data-tab value to a paragraph of text.
    // Edit the sentences on the right side of each colon to your own words.
    const tripDescriptions = {
      all:     "A collection of photos and videos from everywhere I've traveled.",
      korigad: "A rainy monsoon trek up Korigad Fort, thick fog rolling in the whole way up.",
      lohagad: "A day trip to Lohagad — ancient stone steps and a view worth the climb.",
      manali:  "A winter trip to Manali, chasing snow and warm cups of tea.",
      goa:     "A relaxed few days in Goa — beaches, sunsets, and good food."
    };
    // ======================================================================

    const tabs = document.querySelectorAll('.tab');
    const items = document.querySelectorAll('.item');
    const tripDescEl = document.getElementById('tripDesc');
    const galleryEl = document.getElementById('gallery');

    tabs.forEach(tab => {
      tab.addEventListener('click', () => {
        tabs.forEach(t => t.classList.remove('active'));
        tab.classList.add('active');

        const chosen = tab.getAttribute('data-tab');

        // Update the description text above the grid
        tripDescEl.textContent = tripDescriptions[chosen] || "";

        // "All" tab: hide every photo/video, show description only
        if (chosen === 'all') {
          galleryEl.classList.add('hidden');
        } else {
          galleryEl.classList.remove('hidden');
        }

        // Show or hide items to match the chosen tab
        items.forEach(item => {
          const category = item.getAttribute('data-category');
          if (category === chosen) {
            item.classList.remove('hidden');
          } else {
            item.classList.add('hidden');
          }
        });
      });
    });

    // Video popup player logic
    const lightbox = document.getElementById('lightbox');
    const lightboxFrame = document.getElementById('lightboxFrame');

    items.forEach(item => {
      const videoId = item.getAttribute('data-video');
      if (videoId) {
        item.addEventListener('click', () => {
          lightboxFrame.src = 'https://www.youtube.com/embed/' + videoId + '?autoplay=1&mute=1';
          lightbox.classList.add('open');
        });
      }
    });

    function closeLightbox() {
      lightbox.classList.remove('open');
      lightboxFrame.src = '';
    }

    document.getElementById('closeBtn').addEventListener('click', closeLightbox);
    lightbox.addEventListener('click', (e) => {
      if (e.target === lightbox) closeLightbox();
    });

  </script>

</body>
</html>
