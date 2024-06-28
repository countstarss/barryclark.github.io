# WhatsApp Clone




## commit 34 showing chat partner information in group chats

###  控制是否在BubbleText中显示用户头像
- 使用一个变量控制
- 变量来源于MessageItem，在MessageItem中添加isGroupChat：Bool

- 把页面padding逻辑放到MessageItem属性里边，UI只负责渲染
- 添加leadingPadding和trailingPadding属性

- 由于初始化时图片链接是可选的，所以可传可不传，当然首选是图片url，而不是fallbackImage
- CircularProfileImageView(item.sender.profileImageUrl,size: .mini)
- 图片链接在UserItem中，直接将UserItem作为一个属性添加到MessageItem中


## commit 36 Fetching channel members from ChatRoom

###  这一部分主要是获取channel中的members
###  现在已经有了currnetUser和前两个用户，只需要获取其他user
###  只取前两个用户的历史原因应该是因为要设置群组名字
        取出两个名字作为channelMembers
            根据members封装membersExcludingMe
                使用map从membersExcludingMe中取出fullName数组
                    判断groupChannel的人数，按照不同的规则拼接groupMembersNames


## commit 38 删除选中的图片媒体文件

- 首先给enum类型的userAction添加一个case：remove
- 然后把 ChatRoomViewModel 中的 handleMediaAttachmentPreview 补全，添加上remove对应的操作：remove（）
- 在 ChatRoomViewModel 中新建函数 remove（） ，添加参数 item : MediaAttachment ,因为要根据这个item的id去匹配要删除的数组的内容
- 填充remove（），思路：找到要删除的attachment 的 index
  - attachmentIndex是 mediaAttachments 中的$0.id 和 item.id匹配的第一项的index 
  - 根据index，删除内容 mediaAttachments.remove（at: attachmentIndex）
  - ======同理，删除photoPicker选中的内容======
  - photoIndex 是 photoPickerItems 中的$0.ItemIdentifier 和 item.id匹配的第一项的index
- ItemIdentifier是photoPicker中特有的内容，实际上添加到photoPicker数组中的是这些图片的ItemIdentifier
  - 这也是为什么完成上一步之后，会出现可以删除thumbnail，但是进入photoPicker，没有取消选中
  - 但是现在我们添加到数组里的是自动生成的UUID，这也是priview和photoPicker不能同步的原因
  - 把UUID替换成ItemIdentifier
- 一般来说，我们无法访问photoPicker，所以要先把photoLibrary分享出来，在添加photoPicker的地方添加 photoLibrary: .shared()
### 到这里，点击cancle按钮删除媒体文件的逻辑处理完成，点击按钮就可以传导到对应的位置完成操作

### 添加动画

- 目前删除内容会两个bug，一个是删除的时候略缩图会重新加载，另外一个是点击的时候背景会闪
- 造成第一个现象的原因是我们的加载方式
- 导致第二个的原因是 ChatRoomViewModel 中控制preview显示的计算属性
  - 它判断attachment是否为空来控制显示，但是实际上有两个容器，所以添加一个 || photoPickerItems.isEmpty

- 完成这些之后，添加一些动画效果，也就是使用
  - .animation（.easeInOIut，value: viewModel.showPhotoPickerPreview）


## commit 40 & 41 : Audio Recoder

### 录音Service
- 1.Setup audioSession
- 2.设置文件目录
- - 设置存储位置
- - 添加Date扩展，生成录音文件名字
- 3.Recoder Settings
- 4.开始录音
- 5.加入计时器
- 6.设置计时器
- 7.停止录音
- - 关闭录音服务
- 8.给stopRecording添加completion
- - 确定completion需要的参数，因为我们最后要把录音文件的url给firebase用来上传文件到存储库，并且要在页面上显示录音的时长，所以要回传的参数是audioUrl和audioDuration
- 9.清除无用的录音文件
- - tearDown()
- - - deleteRecordings(_ *urls:[ URLS ])*
  - - - deleteRecordings(at: fileUrl:URL )

## PART 2
### 现在已经完成了语音消息的录制和预览

### 把录音时长传递到TextInputArea
- 给TextInputArea添加变量，用于填充录音时间
- 在viewModel中创建对应的变量，绑定起来
- - isRecordingVoiceMessage
  - elapsedVoiceMessageTime
- @Published private(set) VoiceService中的控elapsedTim读
- 在主线程上监听是否开始录制
- 把线程监听函数添加到init()
- 在TextInputArea中通过onChanged监听绑定的变量isRecording，控制isPlusing的动画，这个时候就不需要toggle了，通过监听直接改变true和false的值
- 插入录音时长变量，因为格式不对，所以需要给添加TimeInterval添加extension,返回我们想要的格式
- - format (%2d:%2d,min,secsec
- 在ChatRoomViewModel的deinit中调用tearDown()，也就是在退出当前对话时，会删除掉所有发送的信息
- 在VoiceService的deinit中也调用一下
- 在tearDown开头加一个判断，如果离开的时候正在录音，那么就stopRecord,然后继续执行，把存的录音都删掉
### 解决录音之后，再添加图片，录音文件消失的问题
- 主要是因为我们在再次添加的时候删除了所有的mediaAttachment
- 修改方式，让audioAttachment接收过滤器过滤的类型为audio的数据
- 然后把这个数据再传回mediaAttachment,再次进入photoPicker，会把图片添加在这个基础上，解决了之前的问题
### 在录音时禁用photoPicker
- 把disable的条件写成isRecording
- 再加上一个grayScale，如果录音，就变灰色

## Commit 42 
 
- 创建一个Firebasehelper
  - task:上传不同种类的图片文件
- 定义Extension,enum类型的UploadType
- 定义Completion，用来回传想要的返回值
  - 传URL和Error
  - 传上传的进度
- 定义enum错误类型和UploadError的extension

### uploadImage
- 参数： image，type，completion，progressHandler
- 1. 拿到文件的存储路径
- 2. 向路径上传文件，使用uploadTask接收
  - 如果有error，处理error
  - 否则上传文件
- 3. 观察uploadTask，记录进度，最后progressHandler(percentage)

### uploadFile


## Episode 47 Sending Photo Messages
## Next

disbaleSendButton
### 处理按钮问题，让有media文件或者photo的时候，按钮也处于可以使用的状态

### 处理发送图片的问题
- 首先修改sendMessage函数，如果媒体文件为空，那么直接发送textMessage
- 如果有媒体文件，走另外一个分支

- ### 发送多条媒体消息 参数：text attachment
  - 选择消息类型 photo video audio

- ### 发送图片消息
  - 把数据加载到storage -- uploadImageToStorage

- ### uploadImageToStorage 参数：attachment completion（imageUrl）
  - 上传文件之后拿到imageUrl，然后发送到数据库
  - 这里需要向MessageService里面添加内容
  - (MessageService包含发送消息、接受消息等等)
    - 发送消息: 将消息存储到数据库
    - 接收消息: 从数据库中获取消息 传回messages,也就是[Messageitem]

- ### sendMediaMessage 参数： channel params completion（）
  - 

























