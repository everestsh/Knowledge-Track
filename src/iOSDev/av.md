# 音视频

积跬步，行千里。新领域，心创造。

- [bilibili/ijkplayer](https://github.com/bilibili/ijkplayer) - 基于 FFmpeg 的iOS/安卓视频播放器
- [GPUImage](https://github.com/BradLarson/GPUImage2) - 基于 OpenGL ES 的 GPU 加速视频/图片处理框架

## 链接

- [Cabbage](https://github.com/VideoFlint/Cabbage/wiki/%E4%B8%AD%E6%96%87%E8%AF%B4%E6%98%8E) - iOS 视频编辑核心架构
- [NextLevel](https://github.com/NextLevel/NextLevel) - Rad Media Capture in Swift
- [Professional Video Applications - Apple Developer Documentation](https://developer.apple.com/documentation/professional_video_applications)

### 技术说明文档

- [Incorporating HDR video with Dolby Vision into your apps](https://developer.apple.com/av-foundation/Incorporating-HDR-video-with-Dolby-Vision-into-your-apps.pdf) - 支持杜比视界的 HD

### 学习资源

- [雷霄骅的专栏](https://blog.csdn.net/leixiaohua1020) - 「中国传媒大学一个搞广播电视相关的视音频技术的学生」🌟🌟🌟
- [音视频开发进阶](https://glumes.com/) - 一个抖音多媒体开发工程师的技术交流分享
### 文章

- [Camera and Photos](https://www.objc.io/issues/21-camera-and-photos/) - Objc.io 相机与照片（[中文](https://objccn.io/issue-21-0/)）
  - [相机工作原理](https://objccn.io/issue-21-1) ✅
  - [图片格式](https://objccn.io/issue-21-2)  ✅
  - [iOS 上的相机捕捉](https://objccn.io/issue-21-3)  ✅
  - [照片框架](https://objccn.io/issue-21-4)  ✅
  - [照片扩展](https://objccn.io/issue-21-5)  ✅
  - [Core Image 介绍](https://objccn.io/issue-21-6)  ✅
  - [GPU 加速下的图像处理](https://objccn.io/issue-21-7)  ✅
  - [GPU 加速下的图像视觉](https://objccn.io/issue-21-8)  ✅
  - [基于 OpenCV 的人脸识别](https://objccn.io/issue-21-9)  

- [《揭秘微视——视频特效与非线性编辑技术内幕》](https://zhuanlan.zhihu.com/p/38469443) ✅

- [Dispatches From Darkroom - Medium](https://medium.com/the-bergen-company) - Darkroom 博客专栏
  - [Improving Your Darkroom Editing Experience](https://medium.com/the-bergen-company/improving-your-darkroom-editing-experience-f0ddb66b689e)
    <details>
            <summary>笔记概览</summary>
  
    - **图片加载**
      - 性能优化
      - 通过预加载，避免不必要的低分辨预览图展示
      - 交互上明确体现相册本地加载和 iCloud 下载的差异
      - 其他优化：
        - 编辑图片时，暂停图片库操作，提供更流畅的交互体验
        - 所有图片视图共享同一个图片加载器，确保实时更新、资源共享与性能
        - 降低缓存层内存占用量
    - **RAW + JPG 图片支持**
      - 提供默认加载选项
      - 角标标识 RAW 或 JPG
    - **裁切和变换**
      - 优化横纵比选项
    - **相册应用扩展**
      - 导出性能优化
      - 视频支持 HDR & 缩略图优化
      - UI 交互优化：双击图片直接添加
    </details>

### 经验分享

- [音视频开发工作经验分享](https://www.bilibili.com/video/BV1p54y1X7fY) - 分享从 Android 客户端转到字节跳动音视频 SDK 开发的经验
  - 音视频开发工作主要是做什么
  - 如何点亮音视频技能树
  - 音视频岗位的转行和面试
