usbfilter kernel

Distro: Ubuntu 14.04 LTS

'uname -a': Linux arpsec04 3.13.11-usbfilter #12 SMP Thu Jul 2 11:30:29 PDT 2015 x86_64 x86_64 x86_64 GNU/Linux

Changes:


20. drivers/usb/core/usb.c                      // инициализация и/или отключение работы usbfilter вместе с USB ядром

3.  include/linux/usbfilter.h                   // основная часть реализации usbfilter
7.  drivers/usb/Kconfig                         //
8.  drivers/usb/Makefile                        //
12. drivers/usb/filter/Kconfig                  //
13. drivers/usb/filter/Makefile                 //
19. drivers/usb/filter/usb_filter.h             //
14. drivers/usb/filter/filter.c                 //


15. include/linux/usb.h                         // добавил поля для номера интерфейса в энпоинте и PID приложения в URB
26. include/linux/skbuff.h                      // добавил в поле sk_buff поле для PID приложения


9.  drivers/usb/core/hcd.c                      // фильтрация URB перед его внедрением в очередь задач? и дебаг принты
21. drivers/staging/usbip/vhci_hcd.c            // фильтрация URB перед оправкой по сети интернет


24. drivers/usb/core/config.c                   // сохранить номер интерфейса в структуре эндпоинта
27. net/ipv4/ip_output.c                        // сохранение PID в структуру skb


16. drivers/usb/core/urb.c                      // вставить PID в URB перед делегированием его передачи методу usb_submit_urb [hcd.c]
22. drivers/net/usb/usbnet.c                    // получает PID приложения через буффер skb для URB (как я понял во время получения данных)
23. drivers/net/wireless/rt2x00/rt2x00usb.c     // получает PID приложения через буффер skb для URB при TX и RX
17. drivers/usb/storage/transport.c             // устанавливает PID приложения для всех URB перед опракой списка scatter-gather по bulk трансферу


1. include/linux/blkdev.h    // используются depricated функции, дебажная инфа и установка PID в структуру запроса

2. include/linux/elevator.h  // мерджит (или только проверяет на возможность мерджа) реквесты в структуру блочных I/O
5. block/elevator.c          //


4. block/blk-core.c                             // непонятная херота



6. drivers/scsi/sd.c                   // просто дебажные принты рабоыт SCSI функций над USB (какое имя девайса, чето там про новый реквест)
10. drivers/usb/storage/usb.c          // дебаг принты
11. drivers/usb/storage/scsiglue.c     // дебаг принт перед постановкой в очередь
18. drivers/usb/storage/uas.c          // дебаг принты в функции USB Attached SCSI (UAS)
25. drivers/usb/core/hub.c             // дебаг принт чтобы поянть, как быстро происходит иницилизация нового устройства с включенным usbfilter

Jul 2, 2015

root@davejingtian.org

http://davejingtian.org
