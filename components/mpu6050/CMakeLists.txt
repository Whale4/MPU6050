idf_component_register(SRCS "mpu6050_application.c" "kalman_filter.c"
                       INCLUDE_DIRS "include"
                       REQUIRES i2c)