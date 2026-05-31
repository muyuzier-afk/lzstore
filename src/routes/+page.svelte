<script lang="ts">
  import { onMount } from 'svelte'
  import { goto } from '$app/navigation'
  import { page } from '$app/stores'

  let authorized = $state(false)
  let checking = $state(true)
  const VALID_TOKEN = '52d72e02-d89f-457a-98c0-b4a2a0571a37'

  const products = [
    { id: 'hecheng-30', series: '和成天下', name: '和成天下 30 元', price: 33, wholesale: 12.41, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B30.jpg' },
    { id: 'hecheng-50', series: '和成天下', name: '和成天下 50 元', price: 55, wholesale: 21.98, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B50.jpg' },
    { id: 'hecheng-100', series: '和成天下', name: '和成天下 100 元', price: 110, wholesale: 46.2, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B100.jpg' },
    { id: 'hecheng-200', series: '和成天下', name: '和成天下 200 元', price: 220, wholesale: 66, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B200.jpg' },
    { id: 'kouweiwang-30', series: '口味王', name: '口味王 30 元', price: 33, wholesale: 16.37, min: 1, image: '/products/%E5%8F%A3%E5%91%B3%E7%8E%8B30.jpg' },
    { id: 'wulong-10', series: '乌龙茶香', name: '乌龙茶香 10 元', price: 11, wholesale: 5.48, min: 2, image: '/products/%E4%B9%8C%E9%BE%99%E8%8C%B6%E9%A6%9910.jpg' },
    { id: 'wulong-2', series: '乌龙茶香', name: '乌龙茶香 2 元', price: 2.2, wholesale: 1.54, min: 20, image: '/products/%E4%B9%8C%E9%BE%99%E8%8C%B6%E9%A6%992.jpg' },
    { id: 'wulong-100', series: '乌龙茶香', name: '乌龙茶香 100 元', price: 110, wholesale: 27.5, min: 2, image: '/products/%E4%B9%8C%E9%BE%99%E8%8C%B6%E9%A6%99100.jpg' }
  ]

  function goToDetail(productId: string) {
    goto(`/product/${productId}`)
  }

  onMount(() => {
    const stored = localStorage.getItem('authorized')
    if (stored === VALID_TOKEN) {
      authorized = true
      checking = false
      return
    }
    
    const token = $page.url.searchParams.get('token')
    
    if (token === VALID_TOKEN) {
      authorized = true
      localStorage.setItem('authorized', VALID_TOKEN)
    } else {
      goto('https://www.baidu.com')
    }
    
    checking = false
  })
</script>

{#if checking}
  <p>验证中...</p>
{:else if authorized}
  <div class="container">
    <div class="header">
      <h1>博康槟榔专卖店</h1>
      <p class="subtitle">海南一手货源 · 正品保障 · 全国包邮</p>
    </div>
    
    <div class="product-grid">
      {#each products as product}
        <div class="product-card" onclick={() => goToDetail(product.id)}>
          <div class="card-image">
            {#if product.id === 'hecheng-30'}
              <span class="card-badge main">主推爆款</span>
              <span class="card-badge discount">量大优惠</span>
            {:else}
              <span class="card-badge">热销</span>
            {/if}
            <img src={product.image} alt={product.name} />
          </div>
          <div class="card-info">
            <h3>{product.name}</h3>
            <p class="series">{product.series}</p>
            {#if product.id === 'hecheng-30'}
              <p class="desc highlight">🔥 店长主推！性价比之王，口感醇厚，日常必备。批发 5 件以上享额外折扣，速抢！</p>
            {:else}
              <p class="desc">海南一手货源，正品保障。口感醇厚，提神醒脑，批发优惠中。</p>
            {/if}
            <div class="price-row">
              <span class="price">¥{product.wholesale}</span>
              <span class="min">{product.min}件起</span>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 100%;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Microsoft YaHei', sans-serif;
      background: #f6f6f6;
      min-height: 100vh;
    }

    .header {
      background: #e1251b;
      padding: 25px 15px 20px;
      text-align: center;
    }

    h1 {
      color: #fff;
      font-size: 22px;
      font-weight: 700;
      margin-bottom: 6px;
      letter-spacing: 1px;
    }

    .subtitle {
      color: rgba(255,255,255,0.9);
      font-size: 12px;
    }

    .product-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1px;
      background: #ebebeb;
    }

    .product-card {
      background: #fff;
      cursor: pointer;
      display: flex;
      flex-direction: column;
    }

    .product-card:active {
      background: #f8f8f8;
    }

    .card-image {
      width: 100%;
      aspect-ratio: 1;
      background: #f8f8f8;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      padding: 15px;
    }

    .card-image img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }

    .card-badge {
      position: absolute;
      top: 8px;
      left: 8px;
      background: #ff4444;
      color: white;
      padding: 2px 8px;
      border-radius: 4px;
      font-size: 10px;
      font-weight: 600;
    }

    .card-badge.main {
      background: linear-gradient(135deg, #ff4444 0%, #ff2222 100%);
      font-size: 11px;
      padding: 3px 10px;
      font-weight: 700;
    }

    .card-badge.discount {
      position: absolute;
      top: 34px;
      left: 8px;
      background: linear-gradient(135deg, #ff6b6b 0%, #ee5a5a 100%);
      font-size: 10px;
      padding: 2px 7px;
      z-index: 2;
    }

    .card-info .desc {
      margin: 0 0 10px 0;
      font-size: 11px;
      color: #666;
      line-height: 1.5;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    .card-info .desc.highlight {
      color: #ff4444;
      font-weight: 500;
      background: #fff5f5;
      padding: 6px 8px;
      border-radius: 4px;
      margin: 0 0 10px 0;
    }

    .card-info {
      padding: 12px 10px;
      flex: 1;
      display: flex;
      flex-direction: column;
      gap: 6px;
      min-height: 140px;
      position: relative;
      z-index: 10;
    }

    .card-info .series {
      margin: 0;
      font-size: 11px;
      color: #999;
      background: #f5f5f5;
      padding: 2px 6px;
      border-radius: 3px;
      display: inline-block;
      order: 1;
    }

    .card-info .desc {
      margin: 0;
      font-size: 11px;
      color: #666;
      line-height: 1.5;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
      order: 2;
    }

    .card-info .desc.highlight {
      color: #ff4444;
      font-weight: 500;
      background: #fff5f5;
      padding: 6px 8px;
      border-radius: 4px;
      margin: 0;
      order: 2;
    }

    .card-info h3 {
      margin: 0 0 6px 0;
      font-size: 14px;
      color: #333;
      font-weight: 500;
      line-height: 1.4;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
      order: 0;
    }

    .card-info .price-row {
      margin-top: auto;
      order: 3;
    }

    .card-info h3 {
      margin: 0 0 6px 0;
      font-size: 14px;
      color: #333;
      font-weight: 500;
      line-height: 1.4;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    .card-info .series {
      margin: 0 0 8px 0;
      font-size: 11px;
      color: #999;
      background: #f5f5f5;
      padding: 2px 6px;
      border-radius: 3px;
      display: inline-block;
    }

    .card-info .desc {
      margin: 0 0 10px 0;
      font-size: 11px;
      color: #666;
      line-height: 1.5;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    .price-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .price-row .price {
      font-size: 20px;
      font-weight: 700;
      color: #e1251b;
    }

    .price-row .min {
      font-size: 11px;
      font-weight: 500;
      color: #fff;
      background: #e1251b;
      padding: 3px 8px;
      border-radius: 10px;
    }

    @media (max-width: 768px) {
      h1 {
        font-size: 20px;
      }
      
      .subtitle {
        font-size: 11px;
      }
      
      .card-info h3 {
        font-size: 13px;
      }
      
      .price-row .price {
        font-size: 18px;
      }
    }
  </style>
{/if}
