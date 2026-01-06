---
tags:
  - C
data: 2025-11-24T21:33:00
---
## 初始化
- ==SDL_Init(SDL_INIT_VIDEO | SDL_INIT_AUDIO)==
- 成功就返回0，失败就返回负数
- ==SDL_Log("")==(打印函数)
- ==SDL_GetError()==返回错误信息的字符串
- ==SDL_Quit()==退出
- ==SDL_CreateWindow(const char\*title, int x, iny y, int w, int h,Uint32 flages)==(左上角为原点，w/h:宽/高)
- 默认定义：SDL_WINDOWPOS_CENTERED----居中
- Uint32 flages: 一般输入0
>SDL_WINDOW_FULLSCREEN:全屏
>SDL_WINDOW_HIDDEN:隐藏
>SDL_WINDOW_BORDERLESS:去除边框
>SDL_WINDOW_RESIZABLE:可改变大小的
>SDL_WINDOW_MAXIMIZED:最大化
   SDL_WINDOW_MINIMIZED:最小化
```c
#include<stdio.h>
#include<SDL.h>
SDL_Window *win = SDL_CreateWindow("Hello", SDL_WINDOWPOS_CENTERED,SDL_WINDOWPOS_CENTEWED,600,400,0);
if(NULL == win){
    SDL_Log("Init Failed:%s",SDL_GetError());
    return -1;
}
SDL_DestroyWindow(win);
SDL_Quit();
return 0;
```
- ==SDL_DestroyWindow(SDL_Window \*)==销毁窗口
- ==SDL_Delay(int)==暂停窗口int 接受毫秒
- A
## 事件监听与响应
- ==SDL_Event==类型
- ==SDL_PollEvent(SDL_Event \*)==有事件：返回1；无事件：返回0
- SDL_SetWindowSize(win,w,h)设置窗口的大小
```c
SDL_Event event;
while (1){
    if (SDL_PollEvent(&event)){
        if(event.type == SDL_QUIT){
            break;
        }
    }
}
```
## 在窗口上绘制一个矩形
```C
SDL_Surface *surf = SDL_GetWindowSurface(win);
SDL_Rect rect = {0,0,100,100};
SDL_FillRect(surf, &rect,0xff0000);
SDL_UpdateWindowSurface(win);
```
- ==SDL_GetWindowSurface(SDL_Window \*)==成功：返回指针；失败：返回NULL
- ==SDL_FreeSurface(SDL_Surface \*)==释放
- ==SDL_Rect = {int x,int y,int w,int h}==区域类型
- ==SDL_FillRect(SDL_Surface \*, const SDL_Rect \*, Uint32 color)==绘制矩形
- ==SDL_FillRects(SDL_Surface \*, const SDL_Rect \*,  int count, Uint32 color)==绘制多个矩形
>color: 十六进制----0xff0000（红，绿，蓝）
>或者分开：==SDL_MapRGB(surf->format,255, 0, 0)==
- ==SDL_UpdateSurface(SDL_Window \*)==更新屏幕
>SDL_PointInRect(&pt,&rect)
### 显示BMP图片
- 1.获取和窗口相关的surface
```C
SDL_Surface *surf = SDL_GetWindorSurface(win);
if(NULL = surf)
{
	SDL_Log("SDL_GetWindowSurface failed: %s",SDL_GetError());
	return -1;
}
```
- 2.倒入BMP图片
```c
SDL_Surface *bmp_surf = SDL_LoadBMP("8.bmp");
if(NULL = surf)
{
	SDL_Log("SDL_LoadBMP failed: %s",SDL_GetError());
	return -1;
}
```
- 3.将图片Surface复制到窗口Surface上 ，并更新
```C
SDL_BlitSurface(bmp_surf,NULL,surf,NULL);
SDL_UpdateWindowSurface(win);
```
>NULL代表整体（否则为SDL_Surface* 类型的区域）
- 4.释放surface
```C
SDL_FreeSurface(bmp_surf);
```
### 单独修改像素点的颜色
```C
typedef struct SDL_Surface
{
	Uint32 flags;
	SDL_PixelFormat *format;
	int w,h;
	int pitch;
	void *pixels;
	void *userdate;
	int locked;
}
```
- 内存中，四个字节存储一个像素点的元素
```C
Uint32 *px = (Uint32*)surf->pixels;
px[20000] = 0xffffff;
//记得更新
```
>Uint32:四字节类型
### 渲染器
- 1.创建一个渲染器
```C
SDL_Renderer *rdr = SDL_CreateRender(win,-1,0);
if语句检测是否成功
```
- 2.设置渲染颜色
```C
SDL_SetRenderDrawColor(rdr,0,255,0,255);
```
- 3.清楚屏幕
```C
SDL_RenderClear(rdr);
```
- 4.渲染呈现
```C
SDL_RenderPresent(rdr);
```
- 5.销毁渲染器
```C
SDL_DestroyRenderer(rdr);
```
### 渲染点，线
```C
SDL_SetRenderDrawColor(rdr,0,0,0,255);
SDL_RenderDrawPoint(rdr,200,200);//（200，200）坐标
SDL_RenderPresent(rdr);
```
```C
SDL_RenderDrawline(rdr,0,0,200,255);
SDL_RenderPresent(rdr);
```
```C
SDL_Point pt[3] = {{0,0},{100,100},{100,300}};
SDL_RenderDrawLines(rdr,pt,3);
SDL_RenderPresent(rdr);
```
### 渲染矩形
```C
SDL_RenderDrawcolor(rdr,0,0,0,255);
SDL_Rect rect = {200,200,100,100};//(x,y,w,h)
SDL_RenderDrawRect(rdr,&rect);//只是画
SDL_RenderFillRect(rdr,&rect);
```
- **使他透明**
```C
SDL_RenderClearDrawBlendMode(rdr,SDL_BLENDMODE_BLEND);
SDL_RenderDrawcolor(rdr,0,0,0,100);//数值越低越透明
SDL_Rect rect = {200,200,100,100};
SDL_RenderDrawRect(rdr,&rect);//只是画
SDL_RenderFillRect(rdr,&rect);
```
### 渲染图片
- 1.导入图片
```C
SDL_Surface *img_surf = SDL_LoadBMP("../SDL/img/2.bmp");
```
- 2.创建texture
```C
SDL_Texture *texture = SDL_CreateTextureFromSurface(rdr,img_surf);
```
>记得检验是否创建成功
- 3.复制texture
```c
SDL_RenderCopy(rdr,texture,NULL,NULL)
SDL_RenderCopyEx(rdr,texture,NULL,&rect,0,NULL,SDL_FLIP_NONE);
SDL_Point pt = {0,0};//是指相对于rect的（0，0）
SDL_RenderCopyEx(rdr,texture,NULL,&rect,0,&pt,SDL_FLIP_NONE);
//0:旋转角度；NULL：旋转中心（NULL默认为第四个参数的中心）；最后：翻转效果
//SDL_FLIP_NONE:不反转；SDL_FLIP_HORIZONTAL:水平翻转；SDL_FLIP_VERTICAL
```
>SDL_Rect rect = {100,200,400,600}
>若第一个NULL取了&rect:复制该部分区域
>若第二个NULL取了&rect：将图片复制到该区域

- 4.渲染呈现
```c
SDL_RenderPresent(rdr);
```
- 5.销毁texture
```c
SDL_DestroyTexture(texture);
```
### 窗口事件
```c
void event_loop(){
	SDL_Event event;
	while(true){
		while (SDL_PollEvent(&event)){
			switch(event.type){
				case SDL_QUIT:
				return;
				case SDL_WINDOWEVENT:
				if (event.window.event == SDL_WINDOWEVENT_MOVED)
				{
					SDL_Log("window moved");
				}
				
				if (event.window.event==SDL_WINDOWEVENT_SIZE_CHANGED)
				{
					SDL_Log("window size changed");
					draw();  //自己写的绘制函数
				}
			}
		}
	}
}
```
### 鼠标移动事件
```C
void event_loop(){
	SDL_Event event;
	while(true){
		while (SDL_PollEvent(&event)){
			switch(event.type){
				case SDL_QUIT:
				return;
				case SDL_MOUSEMOTION:
				SDL_Log("x=%d,y=%d",event.motion.x,event.motion.y);
				break;
				
				case SDL_MOUSEBUTTONDOWN:
				event.button.x,event.button.y;//按下时的坐标
				event.button.button;//按下的按键：1左2中3右
				event.button.clicks;//连点次数
				//同理有SDL_MOUSEBUTTONUP
			}
		}
	}
}
```
### 键盘事件
```c
void event_loop(){
	SDL_Event event;
	while(true){
		while (SDL_PollEvent(&event)){
			switch(event.type){
				case SDL_QUIT:
				return;
			
				case SDL_KEYDOWN:
				SDL_Log("sym=%d,scancode=%d",event.key.keysym.sym,
				event.key.keysym.scancode);

				if(event.key.keysym.sym == SDLK_UP){
					SDL_Log("UP");
				}
				break;

				case SDL_KEYUP:
				SDL_Log("sym=%d,scancode=%d",event.key.keysym.sym,
				event.key.keysym.scancode);
				break;

				case SDL_TEXTEDITING:

				case SDL_TEXTINPUT:
			}
		}
	}
}
```
### 播放wav音频
```c
Uint8 *audio_buf;//音频数据
Uint32 audio_len;//音频大小
Uint32 audio_pos = 0;//记录当前复制到了哪个位置
SDL_AudioDeviceID device_id;
void callback(void *userdata,Uint8 *stream,int len){
	//SDL_Lod("%s",(char*)userdata);
	int remain = audio_len - audio_pos;
	if (remain > len){
		SDL_memcpy(stream,audio_buf + audio_pos,len);
		audio_pos += len;
	}
	else{
		SDL_memcpy(stream,audio_buf + audio_pos,remain);
		audio_pos = 0;	
	}
}
//会不停的调用这个回调函数
void play_wav()
{
	SDL_AudioSpec audio_spec;
	//1.导入WAV文件
	if(SDL_LoadWAV("1.wav",&audio_spec,&audio_buf,&audio_len)==NULL){
		SDL_Log("SDL_LoadWAV failed:%s",SDL_GetError());
		return;
	}
	//2.定义播放回调函数
	audio_spec.callback = callback;
	audio_spec.userdata = (void *)"这是外部传进来的数据";
	//3.打开音频设备
	device_id = SDL_OpenAudioDevice(NULL,0,&audio_spec,NULL,0);
	                                //0:播放，非零：录音
	        //打开成功：大于0；打开失败：小于0；
	//4.开始播放
	SDL_pauseAudioDevice(device_id,0)//0:播放；1：暂停
	//5.销毁
		
	
}

SDL_FreeWAV(audio_buf);
SDL_CloseAudioDevice(device_id);//记得销毁
```
### 动画帧率控制
```c
int index = 1;

char file[20] = 0;
if(index > 1736){
	return -1;
}
SDL_snprintf(file,20,"img/%d.bmp",index ++);

void event_loop(){
	SDL_Event event;
	Uint64 start,end;
	int delay;
	while (true){
		start = SDL_GetTicks64();
		while (SDL_PollEvent(&event))//同上，不写了
		draw();
		end = SDL_GetTicks64;
		delay = FT - (end-start);
		if(delay>0){
			SDL_Delay(delay);
		}
	}
}
```
### 显示文字
```c
void draw_text(){
	//1.初始化文字
	if(TTF_Init()<0){
		SDL_Log("TTF_Init failed:%s",TTF_GetError());
		return;
	}
	//2.打开字体
	TTF_Font *Font = TTF_OpenFont("C:\\Windows\\Fonts\\simfang.ttf"
	,72);
	if(!font){
		SDL_LOg("TTF_OpenFont failed:%s",TTF_GetError());
		return;
	}
	//3.渲染字体
	SDL_Surface *txt_surf = TTF_RenderText_Solid(font,"Hello"
	,{255,255,255});  //只能打印英文
	SDL_Surface *txt_surf = TTF_RenderUTF8_Solid(...)//可以汉字
	TTF_RenderUTF8_Blended//渲染的更好
	SDL_Surface *txt_surf = TTF_RenderText_Shaded(font,"Hello你好"
	,{255,255,255},{255,0,0,255});  //有背景色
	
	//4.获取与窗口关联的Surface
	SDL_Surface *surf = SDL_GetWindowSurface(win);

	//5.将字体Surface复制到窗口Surface上
	SDL_Rect rect = {500,500,txt_surf->w,txt_surf->h};
	SDL_BlitSurface(txt_surf,NULL,surf,&rect);

	//6.更新窗口
	SDL_UpdateWindowSurface(win);

	//7.释放和销毁资源
	SDL_FreeSurface(surf);
	SDL_FreeSurface(txt_surf);
	TTF_CloseFont(font);
}
```