- 1 deployment nginx (replicas=2 pod)
    ![alt text](image.png)
- 1 nodePort service trỏ tới deployment
    ![alt text](image-1.png)
- thực hiên curl tới nodePort và cho ra kết quả trang web mặc định của nginx
    ![alt text](image-2.png) ![alt text](image-3.png)
