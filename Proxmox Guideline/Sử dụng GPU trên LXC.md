# Nguồn

[https://www.derekseaman.com/2023/04/proxmox-plex-lxc-with-alder-lake-transcoding.html](https://www.derekseaman.com/2023/04/proxmox-plex-lxc-with-alder-lake-transcoding.html)

# Tóm lược

Cân nhắc sử dụng phương án này. Vì container chạy trực tiếp ở host node dưới LXC

Host → container → plex

Thay vì nếu sử dụng VM

Host → passthrough GPU → VM → container → plex

# Áp dụng

Liệu nên sử dụng phương án trên thay vì VM passthrough không?

Khi VM sập hoặc di chuyển có thể sẽ khó khăn hơn?