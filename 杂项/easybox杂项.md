mount无参数对比

![image-20241030191106301](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030191106301.png)

原始 mount 命令显示了额外的选项：

- `x-gdu.hide`
- `x-gvfs-hide`

 easybox mount 命令省略了这些选项，只显示了基本的挂载选项：

- `ro,nodev,relatime,errors=continue,threads=single`









 -a输出不一致

![image-20241030194122489](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030194122489.png)









sudo ./easybox mount -o ro,noexec ext4.img mount_point

sudo mount -o ro,noexec ext4.img mount_point 

挂载flag不一致

![image-20241030194333229](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030194333229.png)









sudo ./easybox mount --bind  source mount_point 会导致mount_point被挂载两次

![image-20241030195049200](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030195049200.png)

sudo ./easybox mount --bind mount_point source 绑定挂载成功

![image-20241030195119837](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030195119837.png)





移动挂载有问题：

![image-20241030195327686](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030195327686.png)





sudo ./easybox mount --no-mtab ext4.img mount_point中mtab还是有相关内容

但是我使用系统mount --no-mtab也一样，mtab里有相关内容





fork输出

![image-20241030200207304](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030200207304.png)







-T输出不一致

![image-20241030200807800](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241030200807800.png)