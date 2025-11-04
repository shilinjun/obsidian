
| ==功能==                   | ==TAG==                            | ==内容==                                                                          |
| ------------------------ | :--------------------------------- | :------------------------------------------------------------------------------ |
| 获取窗口控制项                  | 无                                  | /ISAPI/Bumblebee/Platform/V1/ControlItem                                        |
| 获取加密参数                   | TAG_CENTRAL_GET_SECURITY_CRYPTO_V0 | /ISAPI/Bumblebee/Platform/V0/Security/Crypto                                    |
|                          | TAG_CENTRAL_GET_SECURITY_CRYPTO_V1 | /ISAPI/Bumblebee/Platform/V1/Security/Crypto                                    |
| 客户端登陆 [[hikCentral登录流程]] | TAG_CENTRAL_LOGIN_V0               | /ISAPI/Bumblebee/Platform/V0/Login                                              |
|                          | TAG_CENTRAL_LOGIN_V1               | /ISAPI/Bumblebee/Platform/V1/Login                                              |
| 客户端退出                    | TAG_CENTRAL_LOGOUT                 | /ISAPI/Bumblebee/Platform/V0/Logout                                             |
| 获取验证码                    | TAG_CENTRAL_VERITYCODE             | /ISAPI/Bumblebee/Platform/V0/VerificationCodeImage                              |
| 获取版本信息                   | TAG_CENTRAL_VERSION                | /ISAPI/Bumblebee/Platform/V0/Version                                            |
| 登陆保活                     | TAG_CENTRAL_KEEPLIVE               | /ISAPI/Bumblebee/Platform/V0/KeepLive                                           |
| 获取安全认证Token              | TAG_CENTRAL_TVWALL_TOKEN           | /ISAPI/Bumblebee/Platform/V0/SystemConfig/Token                                 |
| 获取站点信息                   | TAG_CENTRAL_RSM_SITE               | /ISAPI/Bumblebee/Platform/V0/RSM/Sites                                          |
| 获取区域信息                   | TAG_CENTRAL_GET_AREAS              | /ISAPI/Bumblebee/DeviceResource/V1/LogicalResource/Areas                        |
| 获取区域下监控点信息               | TAG_CENTRAL_GET_CAMERAS            | /ISAPI/Bumblebee/DeviceResource/V1/LogicalResource/Elements                     |
| 获取资源MAP信息                | TAG_CENTRAL_GET_CAMSERIAL          | /ISAPI/Bumblebee/Platform/V0/LogicalResource/Elements/SerialNumber              |
| 获取监控点的录像配置               | TAG_CENTRAL_GET_CAM_RECORD_CONFIG  | /ISAPI/Bumblebee/BaseVideo/V0/RecordSetting/RecordConfig                        |
| 获取监控点的录像存储介质信息           | TAG_CENTRAL_GET_CAM_STORAGE_INFO   | /ISAPI/Bumblebee/BaseVideo/V0/StorageInfo                                       |
| 获取资源URL                  | TAG_CENTRAL_COMMON_URL             | /ISAPI/Bumblebee/BaseVideo/V0/PreviewCommonUrl                                  |
|                          | TAG_CENTRAL_COMMON_PLAYBACKURL     | /ISAPI/Bumblebee/BaseVideo/V0/PlaybackCommonUrl                                 |
| 云台命令                     | TAG_CENTRAL_PTZ_CONTINUOUS         | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Continuous              |
|                          | TAG_CENTRAL_PTZ_AUTOPAN            | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/AutoPan                 |
|                          | TAG_CENTRAL_PTZ_AUXCONTROLS        | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/AuxControl              |
|                          | TAG_CENTRAL_PTZ_FOCUS              | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Focus                   |
|                          | TAG_CENTRAL_PTZ_IRIS               | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Iris                    |
|                          | TAG_CENTRAL_PTZ_PRESET_SET         | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Presets                 |
|                          | TAG_CENTRAL_PTZ_PRESET_CALL        | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Presets/%d/Goto         |
|                          | TAG_CENTRAL_PTZ_PATRAL_START       | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Patrols/%d/Start        |
|                          | TAG_CENTRAL_PTZ_PATRAL_STOP        | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Patrols/%d/Stop         |
|                          | TAG_CENTRAL_PTZ_PATTERN_START      | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Patterns/%d/RecordStart |
|                          | TAG_CENTRAL_PTZ_PATTERN_STOP       | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Patterns/%d/RecordStop  |
|                          | TAG_CENTRAL_PTZ_PATTERN_CALL       | /ISAPI/Bumblebee/BaseVideo/V0/PTZCtrl/CameraElements/%d/Patterns/%d/Start       |
| 获取Tvwall                 | TAG_CENTRAL_TVWALL                 | /api/tvwall                                                                     |
| 获取大屏列表                   | TAG_CENTRAL_DLP_LIST               | /ISAPI/Bumblebee/TVWall/V1/DLP                                                  |
| 获取大屏信息                   | TAG_CENTRAL_DLP_INFO               | /ISAPI/Bumblebee/TVWall/V1/DLP/%d                                               |
| 获取解码器信息                  | TAG_CENTRAL_DECODER_INFO           | /ISAPI/Bumblebee/TVWall/V1/Decoder/%d                                           |
| 新建窗口                     | TAG_CENTRAL_NEW_FLOAT              | /ISAPI/Bumblebee/TVWall/V1/DLP/%d/FloatWnd                                      |
| 获取单个窗口                   |                                    | /ISAPI/Bumblebee/TVWall/V1/DLP/%d/FloatWnd/%d                                   |
| 窗口分割                     |                                    | /ISAPI/Bumblebee/TVWall/V1/DLP/%d/FloatWnd/%d/Div                               |
| 窗口移动                     |                                    | /ISAPI/Bumblebee/TVWall/V1/DLP/%d/FloatWnd/%d/Move                              |
| 窗口放大恢复                   |                                    | /ISAPI/Bumblebee/TVWall/V1/DLP/%d/FloatWnd/%d/ZoomOutput                        |
| 批量开窗                     | TAG_CENTRAL_BATCH_FLOAT            | /ISAPI/Bumblebee/TVWall/V1/DLP/%d/BatchFloatWnd                                 |
| 电视墙预览                    | TAG_CENTRAL_REALPLAY               | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/RealPlay                                      |
|                          | TAG_CENTRAL_PREVIEW                | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/Preview                                       |
| 窗口放大缩小                   | TAG_CENTRAL_ZOOM                   | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/Zoom                                          |
| 获取大屏场景信息                 | TAG_CENTRAL_SCENE                  | /ISAPI/Bumblebee/TVWall/V1/Scene                                                |
| 切换场景                     |                                    | /ISAPI/Bumblebee/TVWall/V1/Scene/%d/Switch                                      |
| 保存场景                     |                                    | /ISAPI/Bumblebee/TVWall/V1/Scene/%d                                             |
| 电视墙窗口回放                  | TAG_CENTRAL_PLAYBACK               | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/Playback                                      |
|                          | TAG_CENTRAL_PLAYBACKINFO           | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/PlaybackInfo                                  |
|                          | TAG_CENTRAL_PLAYBACK_SPEEDCTRL     | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/PlaybackSpeedCtrl                             |
|                          | TAG_CENTRAL_PLAYBACK_PAUSE         | /ISAPI/Bumblebee/TVWall/V1/Wnd/%d/PlaybackPause                                 |
| 添加标签                     | TAG_CENTRAL_KEYBOARD_LABELS        | /ISAPI/Bumblebee/BaseVideo/V0/VideoSearch/KeyBoard/Labels                       |
| 记录操作日志                   | TAG_CENTRAL_OPERATE_LOG            | /ISAPI/Bumblebee/Platform/V0/Log/OperationLog                                   |
