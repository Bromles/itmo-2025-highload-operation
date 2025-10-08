1. Задание 1. Systemd (25 баллов)

    - Создайте bash-скрипт `/usr/local/bin/homework_service.sh` с содержанием
   ```bash
   #!/bin/bash
   
   echo "My custom service has started."
   while true; do
     echo "Service heartbeat: $(date)" >> /tmp/homework_service.log
     sleep 15
   done
   ```

   ![](./screenshots/bash_script.png)

    - Создайте systemd unit файл для скрипта, который бы переживал любые обновления системы. Убедитесь, что сервис сам
      перезапускается в случае падения через 15 секунд

   ![](./screenshots/systemd_service.png)

   ![](./screenshots/systemd_restart.png)

    - Запустите сервис и убедитесь, что он работает

   ![](./screenshots/systemd_start.png)

   ![](./screenshots/systemd_logs.png)

    - Используя systemd-analyze, покажите топ-5 systemd unit`ов стартующих дольше всего

   ![](./screenshots/systemd_top_services.png)

2. Задание 2. Межпроцессное взаимодействие (IPC) с разделяемой памятью (20 баллов)

    - На любом языке программирования создайте программу, использующую шареную память (создание, ожидание 60 секунд,
      удаление)

   ![](./screenshots/shmem_c.png)

    - Скомпилируйте и запустите

   ![](./screenshots/shmmem_run.png)

    - Пока программа запущена (60 секунд), проанализируйте вывод - в соседнем терминале запустите `ipcs -m`. Обратите
      внимание на `nattch (number of attached processes)`, проанализируйте вывод

   ![](./screenshots/shmem_ipcs.png)

3. Задание 3. Анализ памяти процессов (VSZ vs RSS) (20 баллов)

    - Откройте 1 окно терминала и запустите питон скрипт, который запрашивает 250 MiB памяти и держит ее 2 минуты

   `python3 -c "print('Allocating memory...'); a = 'X' * (250 * 1024 * 1024); import time; print('Memory allocated. Sleeping...'); time.sleep(120);"`

   ![](./screenshots/python_run.png)

    - Пока скрипт запущен, откройте вторую вкладку, найдите там PID запущенного скрипта и проанализируйте использование
      RSS и VSZ

   `ps -o pid,user,%mem,rss,vsz,comm -p %%YOUR_PID%%`

   ![](./screenshots/python_stats.png)

    - Объясните, почему vsz больше rss, и почему rss далеко не 0

4. NUMA и cgroups (35 баллов)

    - Продемонстрируйте количество NUMA-нод на вашем сервере и количество памяти для каждой NUMA-ноды

   ![](./screenshots/numactl.png)

    - Убедитесь, что вы можете ограничивать работу процессов при помощи systemd

   ![](./screenshots/systemd_stress_150m.png)

    - Запустите
   ```bash
   sudo systemd-run --unit=highload-stress-test --slice=testing.slice \
   --property="MemoryMax=150M" \
   --property="CPUWeight=100" \
   stress --cpu 1 --vm 1 --vm-bytes 300M --timeout 30s
   ```

   ![](./screenshots/systemd_status_150m.png)

   ![](./screenshots/systemd_stress_50m.png)

   ![](./screenshots/systemd_show_mem.png)

    - Будет ли работать тест, если мы запрашиваем 300М оперативной памяти, а ограничиваем 150М?
    - В соседней вкладке проследите за testing.slice при помощи systemd-cgls. Превысило ли использование памяти 150М?
      Что происходит с процессом при превышении? Попробуйте использовать разные значения

   ![](./screenshots/systemd_cgls.png)

    - Опишите, что делает и для чего можно использовать MemoryMax и CPUWeight
   
   