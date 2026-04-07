# my-first-project
##我的第一个项目，今天学会了用VS code操作GitHub!
#!/bin/bash
IFS='|' read -ra args <<< "1860811275621797251"
for arg in "${args[@]}"; do
  echo "处理参数: $arg"
        bazel run -c opt //experimental/users/chuanyang:download_run  -- --run_name=$arg  --download_camera=true

done