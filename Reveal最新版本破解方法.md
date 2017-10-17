#Reveal最新版本破解方法

首先，直接运行Reveal发现它弹出了这个弹框。

![](http://bbs.iosre.com/uploads/default/original/2X/6/67195cf758f9e81d79e79a59172b6d0debb6024d.png)

看到这个页面，这是一个app启动时的页面，一般是写到类下的applicationWillFinishLaunching方法里边。

我们打开Hopper Disassembler然后把Reveal拖入，等待它分析完后。

![](http://bbs.iosre.com/uploads/default/original/2X/b/bcde4d3ec46286bcfd217d67eec70982377721d4.png)

发现调用了ptrace函数，那我们就先对ptrace函数做下处理。

处理的时候，可以参考https://www.chenghu.me/?p=1350这篇文章，把ptrace函数的传参1F修改为0A。

改完之后，我们就可以使用Hopper Disassembler的动态调试功能，或者Interface Inspector的界面调试。

首先，去找我们之前猜测的applicationWillFinishLaunching方法。

![](http://bbs.iosre.com/uploads/default/original/2X/b/beed2fc9f41c1fbb033adb7f80adc409e55397c4.png)

发现调用了 sub_10000e010(self) 这个子程序。

跟过去发现有两处调用了sub_10000e010这个子程序

![](http://bbs.iosre.com/uploads/default/original/2X/c/ce1ed9d81b9f6681385e2962ec16dc9241c7b30c.png)

猜测这就是弹框的地方，直接在sub_10000e010这个子程序的头部retn，然后跑起来。

发现之前的弹窗果然没有了。

但是有了新的问题，点菜单栏和退出的时候会闪退(异常退出)。

通过动态调试发现，它会执行ud2这个命令。

![](http://bbs.iosre.com/uploads/default/original/2X/2/27fb57b01172995a1e9706e92031295508c7096f.jpg)

百度一下这个指令：

UD2是一种让CPU产生invalid opcode exception的软件指令.  内核发现CPU出现这个异常, 会立即停止运行.


```
void sub_100192ff0(int arg0) {
    var_C0 = r13;
    var_50 = arg0;
    *0x1004d5798 = *0x1004d5798 + 0x1;
    *0x1004d57b0 = *0x1004d57b0 + 0x1;
    if (*0x1004bab68 != 0xffffffffffffffff) {
            swift_once(0x1004bab68, sub_100208920);
    }
    *0x1004d57b8 = *0x1004d57b8 + 0x1;
    r14 = sub_100023780();
    sub_100008a80();
    r15 = sub_100208980(r14, sub_100208920);
    if (r15 != 0x0) {
            rbx = sub_100010860();
            sub_100008a70();
            if (rbx != 0x0) {
                    *0x1004d57c0 = *0x1004d57c0 + 0x1;
                    *0x1004d57c8 = *0x1004d57c8 + 0x1;
                    r14 = var_50;
                    [r14 retain];
                    if (*(int8_t *)(rbx + 0x30) == 0x0) {
                            *0x1004d57d0 = *0x1004d57d0 + 0x1;
                            swift_unknownRetain(r15);
                            rbx = sub_1000251e0();
                            swift_unknownRelease(r15);
                            if ((rbx & 0x1) != 0x0) {
                                    swift_unknownRelease(r15);
                                    [r14 release];
                                    *0x1004d57a0 = *0x1004d57a0 + 0x1;
                                    rbx = 0x0;
                            }
                            else {
                                    *0x1004d57d8 = *0x1004d57d8 + 0x1;
                                    swift_unknownRetain(r15);
                                    *0x1004d57e0 = *0x1004d57e0 + 0x1;
                                    *0x1004d57e8 = *0x1004d57e8 + 0x1;
                                    rbx = [sub_1002df34d() retain];
                                    rdx = *0x1004bef18;
                                    if (rdx == 0x0) {
                                            rdx = sub_100008a10();
                                            *0x1004bef18 = rdx;
                                    }
                                    r12 = static (rbx, type metadata for Swift.String);
                                    sub_1001656b0(r12, type metadata for Swift.String, rdx);
                                    xmm0 = intrinsic_movaps(xmm0, var_200);
                                    var_210 = intrinsic_movaps(var_210, xmm0);
                                    var_68 = var_1F0;
                                    rbx = var_1E8;
                                    xmm0 = intrinsic_movaps(xmm0, var_1E0);
                                    var_220 = intrinsic_movaps(var_220, xmm0);
                                    var_78 = var_1D0;
                                    r13 = var_1C8;
                                    xmm0 = intrinsic_movaps(xmm0, var_1C0);
                                    var_230 = intrinsic_movaps(var_230, xmm0);
                                    var_80 = var_1B0;
                                    r14 = var_1A8;
                                    var_90 = var_1A0;
                                    var_29 = var_198;
                                    xmm0 = intrinsic_movaps(xmm0, var_190);
                                    var_240 = intrinsic_movaps(var_240, xmm0);
                                    var_98 = var_180;
                                    var_2A = var_178;
                                    xmm0 = intrinsic_movaps(xmm0, var_170);
                                    var_250 = intrinsic_movaps(var_250, xmm0);
                                    var_A0 = var_160;
                                    var_2B = var_158;
                                    var_2C = var_148;
                                    var_2D = var_138;
                                    xmm0 = intrinsic_movaps(xmm0, var_130);
                                    var_260 = intrinsic_movaps(var_260, xmm0);
                                    var_2E = var_118;
                                    xmm0 = intrinsic_movaps(xmm0, var_110);
                                    var_270 = intrinsic_movaps(var_270, xmm0);
                                    var_2F = var_F8;
                                    var_30 = var_F7;
                                    var_31 = var_F6;
                                    var_32 = var_E8;
                                    var_40 = var_D8;
                                    var_48 = var_C8;
                                    var_58 = var_150;
                                    var_60 = var_140;
                                    var_70 = var_120;
                                    var_88 = var_100;
                                    var_A8 = var_F0;
                                    var_B0 = var_E0;
                                    var_B8 = var_D0;
                                    if (r12 >= 0x0) {
                                            sub_100008a70();
                                    }
                                    else {
                                            swift_unknownRelease(r12 & 0x7fffffffffffffff);
                                    }
                                    xmm0 = intrinsic_movaps(xmm0, var_210);
                                    var_3B0 = intrinsic_movaps(var_3B0, xmm0);
                                    intrinsic_movaps(var_390, intrinsic_movaps(xmm0, var_220));
                                    intrinsic_movaps(var_370, intrinsic_movaps(xmm0, var_230));
                                    intrinsic_movaps(var_340, intrinsic_movaps(xmm0, var_240));
                                    intrinsic_movaps(var_320, intrinsic_movaps(xmm0, var_250));
                                    intrinsic_movaps(var_2E0, intrinsic_movaps(xmm0, var_260));
                                    intrinsic_movaps(var_2C0, intrinsic_movaps(xmm0, var_270));
                                    sub_1000d2370(&var_3B0);
                                    xmm0 = intrinsic_movaps(xmm0, var_200);
                                    xmm1 = intrinsic_movaps(xmm1, var_1F0);
                                    intrinsic_movaps(xmm2, var_1E0);
                                    intrinsic_movaps(xmm3, var_1D0);
                                    intrinsic_movaps(xmm4, var_1C0);
                                    intrinsic_movaps(xmm5, var_1B0);
                                    intrinsic_movaps(xmm6, var_1A0);
                                    intrinsic_movaps(xmm7, var_190);
                                    intrinsic_movaps(xmm8, var_180);
                                    intrinsic_movaps(xmm9, var_170);
                                    var_460 = intrinsic_movaps(var_460, xmm0);
                                    intrinsic_movaps(var_450, xmm1);
                                    intrinsic_movaps(var_440, xmm2);
                                    intrinsic_movaps(var_430, xmm3);
                                    intrinsic_movaps(var_420, xmm4);
                                    intrinsic_movaps(var_410, xmm5);
                                    intrinsic_movaps(var_400, xmm6);
                                    intrinsic_movaps(var_3F0, xmm7);
                                    intrinsic_movaps(var_3E0, xmm8);
                                    intrinsic_movaps(var_3D0, xmm9);
                                    sub_100052670(&var_460);
                                    rax = sub_1000165c0(&var_200, var_1E8);
                                    rcx = *(*(var_1E8 + 0xfffffffffffffff8) + 0x88) + 0xf;
                                    (*(*(var_1E8 + 0xfffffffffffffff8) + 0x30))(rsp - (rcx & 0xfffffffffffffff0), rax, var_1E8, rcx & 0xfffffffffffffff0);
                                    r13 = (*var_1E0)(var_1E8, var_1E0);
                                    (*(*(var_1E8 + 0xfffffffffffffff8) + 0x20))(rsp - (rcx & 0xfffffffffffffff0), var_1E8);
                                    sub_1000108a0(&var_200);
                                    rbx = var_50;
                                    [rbx release];
                                    swift_unknownRelease_n(r15, 0x2);
                                    if ((r13 & 0x1) == 0x0) {
                                            rax = [rbx action];
                                            rbx = 0x1;
                                            if ((rax != 0x0) && ((ObjectiveC.== infix(rax, @selector(openNewDocumentWindowAndInspectDemoApp:)) & 0x1) != 0x0)) {
                                                    *0x1004d57a8 = *0x1004d57a8 + 0x1;
                                                    r14 = sub_10000de70();
                                                    if (r14 != 0x0) {
                                                            rbx = ObjectiveC._convertObjCBoolToBool([r14 sampleApplicationIsLaunching] & 0xff);
                                                            [r14 release];
                                                            rbx = rbx ^ 0x1;
                                                    }
                                                    else {
                                                            rbx = 0x0;
                                                    }
                                            }
                                    }
                                    else {
                                            *0x1004d57a0 = *0x1004d57a0 + 0x1;
                                            rbx = 0x0;
                                    }
                            }
                    }
                    else {
                            swift_unknownRelease(r15);
                            [r14 release];
                            *0x1004d57a0 = *0x1004d57a0 + 0x1;
                            rbx = 0x0;
                    }
            }
            else {
                    swift_unknownRelease(r15);
                    asm { ud2 };
                    loc_1001936d4();
            }
    }
    else {
            sub_100008a70();
            asm { ud2 };
            loc_1001936ca(rdi);
    }
    return;
}
```

看到了unknownRelease这种关键字，估摸着这软件可能校验什么自己不正常后自己调用ud2指令直接异常。

![](http://bbs.iosre.com/uploads/default/original/2X/e/e26ff1a61f1463ca25f421d7ad4bf324fcf4c00e.png)

发现调用这些校验的也是菜单的东西，于是我直接把这个校验子程序的首部retn掉。

发现还有一堆这样的校验子程序，基本上每个菜单下边都有这种检测的调用。全部retn掉，不让它执行异常，然后就可以正常的打开和使用了。

![](http://bbs.iosre.com/uploads/default/original/2X/6/677c7a59a1a5ff23c31604a81471110a7108090e.jpg)
