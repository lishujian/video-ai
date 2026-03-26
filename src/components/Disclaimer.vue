<template>
  <footer class="disclaimer">
    <div class="container">
      <div class="divider"></div>
      <div class="content">
        <p class="main-text">
          所有资源均来源第三方资源，并不提供影片资源存储，也不参与录制、上传相关视频，视频版权归属其合法持有人所有。
        </p>
        <p class="sub-text">
          若本站的资源无意侵犯了您的版权，请联系我们，我们会及时回复和处理，谢谢！
        </p>
        <div class="admin-contact">
          <span class="label">管理员邮箱：</span>
          <a :href="'mailto:' + email" class="email">{{ email }} </a>
        </div>
      </div>
      <div class="copyright">
        &copy; {{ new Date().getFullYear() }} Video Analysis. All Rights Reserved.
      </div>
    </div>
  </footer>
</template>

<script setup>
const email = 'videoai512@proton.me'
</script>

<style lang="less" scoped>
.disclaimer {
  padding: 40px 20px 60px;
  background: transparent;
  width: 100%;
  box-sizing: border-box;

  .container {
    max-width: 900px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 24px;
  }

  .divider {
    width: 60px;
    height: 4px;
    background: linear-gradient(90deg, #9b7cf4, #c3b4f9);
    border-radius: 2px;
    opacity: 0.3;
  }

  .content {
    text-align: center;
    color: #6b7280;
    font-size: 13.5px;
    line-height: 1.8;
    max-width: 720px;

    .main-text {
      margin-bottom: 8px;
    }

    .sub-text {
      margin-bottom: 20px;
      font-size: 13px;
      opacity: 0.8;
    }

    .admin-contact {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 6px 16px;
      background: rgba(155, 124, 244, 0.05);
      border-radius: 20px;
      border: 1px solid rgba(155, 124, 244, 0.1);
      transition: all 0.2s ease;

      &:hover {
        background: rgba(155, 124, 244, 0.08);
        border-color: rgba(155, 124, 244, 0.2);
        transform: translateY(-1px);
      }

      .label {
        color: #4b5563;
      }

      .email {
        color: #8b5cf6;
        text-decoration: none;
        font-weight: 500;
        
        &:hover {
          text-decoration: underline;
        }
      }
    }
  }

  .copyright {
    font-size: 11px;
    color: #9ca3af;
    letter-spacing: 0.02em;
    text-transform: uppercase;
  }
}

@media (max-width: 600px) {
  .disclaimer {
    padding: 30px 16px 50px;
  }

  .content {
    font-size: 12.5px;
    
    .sub-text {
      font-size: 12px;
    }
    
    .admin-contact {
      padding: 5px 14px;
      font-size: 12px;
    }
  }
}
</style>
