<script lang="ts">
  import { onMount } from 'svelte'
  import { goto } from '$app/navigation'
  import { page } from '$app/stores'

  let authorized = $state(false)
  let checking = $state(true)
  const VALID_TOKEN = '52d72e02-d89f-457a-98c0-b4a2a0571a37'

  let product = $state<any>(null)
  let showQR = $state(false)

  const products = [
    { id: 'hecheng-30', series: '和成天下', name: '和成天下 30 元', price: 33, wholesale: 12.41, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B30.jpg', description: '经典和成天下 30 元装，精选海南优质槟榔鲜果，传统工艺制作。口感醇厚，回味甘甜，日常嚼食提神的理想选择。每件含多小包，独立包装，方便携带分享。适合日常办公、开车、休闲等场景，提神醒脑，口感舒适。' },
    { id: 'hecheng-50', series: '和成天下', name: '和成天下 50 元', price: 55, wholesale: 21.98, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B50.jpg', description: '和成天下中端品质，50 元经典款。槟榔果颗粒饱满，纤维细腻，卤水配方独特。入口柔顺，不锁喉，适合长时间咀嚼。批发性价比之选，零售自用两相宜。' },
    { id: 'hecheng-100', series: '和成天下', name: '和成天下 100 元', price: 110, wholesale: 46.2, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B100.jpg', description: '和成天下高端系列 100 元装，精选海南优质槟榔鲜果，传统秘制卤水浸泡。口感劲道十足，香气浓郁持久，回甘明显。适合商务招待或资深爱好者品鉴。' },
    { id: 'hecheng-200', series: '和成天下', name: '和成天下 200 元', price: 220, wholesale: 66, min: 1, image: '/products/%E5%92%8C%E6%88%90%E5%A4%A9%E4%B8%8B200.jpg', description: '和成天下旗舰产品 200 元礼盒装，严选特等槟榔果，大师级工艺制作。口感层次丰富，前调清香，中调醇厚，后调回甘持久。高端大气，商务送礼首选。' },
    { id: 'kouweiwang-30', series: '口味王', name: '口味王 30 元', price: 33, wholesale: 16.37, min: 1, image: '/products/%E5%8F%A3%E5%91%B3%E7%8E%8B30.jpg', description: '口味王经典 30 元款，独特卤水配方，口感劲道有嚼劲。槟榔果个大饱满，纤维适中，不渣不涩。持久留香，提神效果佳。全国畅销品牌，品质有保障。' },
    { id: 'wulong-10', series: '乌龙茶香', name: '乌龙茶香 10 元', price: 11, wholesale: 5.48, min: 2, image: '/products/%E4%B9%8C%E9%BE%99%E8%8C%B6%E9%A6%9910.jpg', description: '创新乌龙茶香口味 10 元装，将传统槟榔与乌龙茶香完美融合。口感清爽不腻，茶香醇厚，入口先苦后甜。适合喜欢清淡口味或初次尝试槟榔的消费者。' },
    { id: 'wulong-2', series: '乌龙茶香', name: '乌龙茶香 2 元', price: 2.2, wholesale: 1.54, min: 20, image: '/products/%E4%B9%8C%E9%BE%99%E8%8C%B6%E9%A6%992.jpg', description: '乌龙茶香经济小包 2 元装，单包独立包装，干净卫生。茶香清新，口感适中，便携实惠。适合小额试吃、活动赠品或搭配销售。20 件起批，量大从优。' },
    { id: 'wulong-100', series: '乌龙茶香', name: '乌龙茶香 100 元', price: 110, wholesale: 27.5, min: 2, image: '/products/%E4%B9%8C%E9%BE%99%E8%8C%B6%E9%A6%99100.jpg', description: '乌龙茶香高端系列 100 元装，特级乌龙茶提取香精，茶香浓郁纯正。槟榔果与茶香完美融合，口感清爽回甘，层次分明。适合追求新奇口味的高端消费群体。' }
  ]

  onMount(() => {
    const stored = localStorage.getItem('authorized')
    const token = $page.url.searchParams.get('token')
    
    if (stored !== VALID_TOKEN && token !== VALID_TOKEN) {
      goto('https://www.baidu.com')
      return
    }
    
    if (token === VALID_TOKEN) {
      localStorage.setItem('authorized', VALID_TOKEN)
    }

    const pid = $page.url.pathname.split('/').pop()
    product = products.find(p => p.id === pid) || null
    
    authorized = true
    checking = false
  })

  function goBack() {
    goto('/')
  }

  function handleBuy() {
    showQR = true
  }

  function closeQR() {
    showQR = false
  }
</script>

{#if checking}
  <p>验证中...</p>
{:else if authorized && product}
  <div class="container">
    <div class="nav-bar">
      <button class="back-btn" onclick={goBack}>
        <i class="fas fa-arrow-left"></i>
      </button>
      <span class="nav-title">商品详情</span>
      <div class="nav-spacer"></div>
    </div>
    
    <div class="product-detail">
      <div class="detail-image">
        <img src={product.image} alt={product.name} />
      </div>
      
      <div class="detail-info">
        <h1>{product.name}</h1>
        <p class="series">{product.series}</p>
        
        <div class="price-section">
          <div class="current-price">
            <span class="symbol">¥</span>
            <span class="price">{product.wholesale}</span>
          </div>
          <div class="original-price">
            建议零售价 ¥{product.price}
          </div>
        </div>
        
        <div class="info-row">
          <span class="label">起批数量</span>
          <span class="value">{product.min}件</span>
        </div>
        
        <div class="description">
          <h3>商品详情</h3>
          <p>{product.description}</p>
        </div>
        
        <div class="features">
          <div class="feature-item">
            <i class="fas fa-check-circle"></i>
            <span>正品保障</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-truck"></i>
            <span>全国包邮</span>
          </div>
          <div class="feature-item">
            <i class="fas fa-shield-alt"></i>
            <span>售后无忧</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="bottom-bar">
      <div class="service-icons">
        <div class="icon-item">
          <i class="fas fa-store"></i>
          <span>店铺</span>
        </div>
        <div class="icon-item">
          <i class="fas fa-comments"></i>
          <span>客服</span>
        </div>
      </div>
      <button class="buy-btn" onclick={handleBuy}>
        <i class="fas fa-shopping-cart"></i> 立即购买
      </button>
    </div>
  </div>

  {#if showQR}
    <div class="qr-modal" onclick={closeQR}>
      <div class="qr-content" onclick={(e) => e.stopPropagation()}>
        <button class="close-btn" onclick={closeQR}>×</button>
        <h2>微信扫一扫订购</h2>
        <div class="qr-image">
          <img src="/products/qr-code.jpg" alt="订购二维码" />
        </div>
        <p class="qr-desc">扫描二维码添加客服微信<br/>享受更多优惠和服务</p>
      </div>
    </div>
  {/if}

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    .container {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Microsoft YaHei', sans-serif;
      background: #f6f6f6;
      min-height: 100vh;
      padding-bottom: 70px;
    }

    .nav-bar {
      background: #e1251b;
      height: 50px;
      display: flex;
      align-items: center;
      padding: 0 15px;
      position: sticky;
      top: 0;
      z-index: 100;
    }

    .back-btn {
      background: none;
      border: none;
      color: #fff;
      font-size: 18px;
      cursor: pointer;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .nav-title {
      color: #fff;
      font-size: 17px;
      font-weight: 500;
      flex: 1;
      text-align: center;
      margin-right: 40px;
    }

    .nav-spacer {
      width: 40px;
    }

    .product-detail {
      background: #fff;
      margin-bottom: 10px;
    }

    .detail-image {
      width: 100%;
      aspect-ratio: 1;
      background: #f8f8f8;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 20px;
    }

    .detail-image img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }

    .detail-info {
      padding: 20px 15px;
    }

    .detail-info h1 {
      font-size: 20px;
      color: #333;
      font-weight: 600;
      margin-bottom: 8px;
    }

    .detail-info .series {
      font-size: 12px;
      color: #999;
      margin-bottom: 20px;
    }

    .price-section {
      background: #fff5f5;
      border: 1px solid #ffe5e5;
      padding: 20px;
      border-radius: 8px;
      margin-bottom: 20px;
    }

    .current-price {
      display: flex;
      align-items: baseline;
      margin-bottom: 5px;
    }

    .current-price .symbol {
      font-size: 18px;
      color: #e1251b;
      font-weight: 600;
    }

    .current-price .price {
      font-size: 36px;
      color: #e1251b;
      font-weight: 700;
    }

    .original-price {
      font-size: 13px;
      color: #999;
      text-decoration: line-through;
    }

    .info-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px 0;
      border-bottom: 1px solid #f0f0f0;
    }

    .info-row:last-of-type {
      border-bottom: none;
    }

    .info-row .label {
      font-size: 14px;
      color: #666;
    }

    .info-row .value {
      font-size: 14px;
      color: #333;
      font-weight: 500;
    }

    .description {
      margin-top: 20px;
      padding: 15px;
      background: #f8f8f8;
      border-radius: 8px;
    }

    .description h3 {
      font-size: 15px;
      color: #333;
      margin-bottom: 10px;
    }

    .description p {
      font-size: 13px;
      color: #666;
      line-height: 1.8;
    }

    .features {
      display: flex;
      gap: 15px;
      margin-top: 20px;
      padding-top: 20px;
      border-top: 1px solid #f0f0f0;
    }

    .feature-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 5px;
    }

    .feature-item i {
      font-size: 20px;
      color: #e1251b;
    }

    .feature-item span {
      font-size: 11px;
      color: #666;
    }

    .bottom-bar {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: 60px;
      background: #fff;
      display: flex;
      align-items: center;
      padding: 0 15px;
      box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
      border-top: 1px solid #f0f0f0;
    }

    .service-icons {
      display: flex;
      gap: 15px;
      margin-right: 15px;
    }

    .icon-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 3px;
      color: #666;
      font-size: 10px;
    }

    .icon-item i {
      font-size: 20px;
    }

    .buy-btn {
      flex: 1;
      background: #e1251b;
      color: #fff;
      border: none;
      height: 45px;
      border-radius: 23px;
      font-size: 16px;
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }

    .buy-btn:active {
      background: #c82018;
    }

    .qr-modal {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.6);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1000;
      backdrop-filter: blur(4px);
    }

    .qr-content {
      background: #fff;
      border-radius: 20px;
      padding: 40px;
      text-align: center;
      position: relative;
      max-width: 400px;
      width: 90%;
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
      animation: slideUp 0.3s ease;
    }

    @keyframes slideUp {
      from {
        opacity: 0;
        transform: translateY(30px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .close-btn {
      position: absolute;
      top: 15px;
      right: 15px;
      background: none;
      border: none;
      font-size: 32px;
      color: #999;
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s;
    }

    .close-btn:hover {
      background: #f5f5f5;
      color: #333;
    }

    .qr-content h2 {
      margin: 0 0 25px 0;
      font-size: 20px;
      color: #333;
      font-weight: 600;
    }

    .qr-image {
      margin: 0 auto 20px;
      padding: 15px;
      background: #f8f9fa;
      border-radius: 12px;
      display: inline-block;
    }

    .qr-image img {
      width: 220px;
      height: 220px;
      object-fit: contain;
    }

    .qr-desc {
      margin: 0;
      font-size: 14px;
      color: #666;
      line-height: 1.6;
    }
  </style>
{:else}
  <p>产品不存在</p>
{/if}
