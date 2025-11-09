// Service Worker Version, update this string to force users to download new assets
const CACHE_VERSION = 'news-pwa-v1.0.0';

// Cache names for different types of assets
const STATIC_CACHE = `${CACHE_VERSION}-static-assets`;
const NEWS_CACHE = `${CACHE_VERSION}-dynamic-news`;

// Files that define the App Shell, which should always be cached for offline use
const ASSETS_TO_CACHE = [
    '/',
    '/index.html',
    'https://cdn.tailwindcss.com', // Tailwind CSS link
    // Add paths to local CSS, JS, and font files here
];

/***************************
 * 1. INSTALL EVENT
 * Caches the core App Shell assets immediately.
 ***************************/
self.addEventListener('install', (event) => {
    // Perform installation steps
    console.log('[Service Worker] Installing...');
    event.waitUntil(
        caches.open(STATIC_CACHE)
            .then((cache) => {
                console.log('[Service Worker] Caching App Shell');
                // Use catch to prevent install failure if external asset fetching fails
                return cache.addAll(ASSETS_TO_CACHE).catch(err => {
                    console.warn('[Service Worker] Failed to cache some assets (e.g., Tailwind CDN)', err);
                    // Crucially, still resolve the promise for the core files we *can* cache
                    return Promise.resolve();
                });
            })
            .then(() => self.skipWaiting()) // Activates the new worker immediately
    );
});


/***************************
 * 2. ACTIVATE EVENT
 * Cleans up old caches to save user space.
 ***************************/
self.addEventListener('activate', (event) => {
    console.log('[Service Worker] Activating and cleaning up old caches...');
    const cacheAllowList = [STATIC_CACHE, NEWS_CACHE];

    event.waitUntil(
        caches.keys().then((cacheNames) => {
            return Promise.all(
                cacheNames.map((cacheName) => {
                    if (cacheAllowList.indexOf(cacheName) === -1) {
                        // Delete old or unused caches
                        console.log(`[Service Worker] Deleting old cache: ${cacheName}`);
                        return caches.delete(cacheName);
                    }
                })
            );
        }).then(() => {
            // Ensure the Service Worker takes control of the current page immediately
            return self.clients.claim();
        })
    );
});


/***************************
 * 3. FETCH EVENT
 * Determines how to respond to network requests (caching strategies).
 ***************************/
self.addEventListener('fetch', (event) => {
    // Check if the request is for a network resource (not an extension or internal request)
    if (event.request.url.startsWith('chrome-extension://')) {
        return;
    }

    const requestUrl = new URL(event.request.url);
    const isStaticAsset = ASSETS_TO_CACHE.some(asset => event.request.url.includes(asset.replace('/', '')));

    if (isStaticAsset || requestUrl.pathname === '/') {
        // Strategy: Cache-First for App Shell (core assets)
        event.respondWith(
            caches.match(event.request)
                .then(response => {
                    // Return the cached asset, or fetch from network if not in cache
                    return response || fetch(event.request);
                })
                .catch(err => {
                    // This handles scenarios where fetch fails (e.g., completely offline)
                    console.error('[Service Worker] Cache match failed and no network for static asset.', err);
                    // You could serve a dedicated offline page here if needed
                })
        );
    } else if (event.request.method === 'GET' && requestUrl.pathname.startsWith('/api/news')) {
        // Strategy: Network-First with Cache Fallback for dynamic data (fresh news is important)
        event.respondWith(
            fetch(event.request)
                .then(response => {
                    // Clone the response before putting it in cache (a response can only be read once)
                    const responseClone = response.clone();
                    
                    // Only cache successful responses (status 200)
                    if (response.ok) {
                        caches.open(NEWS_CACHE)
                            .then(cache => {
                                cache.put(event.request, responseClone);
                            });
                    }
                    return response;
                })
                .catch(err => {
                    // Network failed, fall back to the dynamic cache for stale data
                    console.log('[Service Worker] Network failed. Falling back to dynamic cache.', err);
                    return caches.match(event.request);
                })
        );
    } else {
        // Default: Network only for all other requests
        return fetch(event.request);
    }
});

