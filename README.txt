使用说明：

1.需要再定时器中断回调函数中调用vKeyDetect()用于检测按键

以STM32F103的TIM5为例（假设已经初始化的TIM5）

```
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)

{

  if(htim->Instance == TIM5 ){

​    for(uint8_t i = 0; i < KEY_NUM; i++){

​      vKeyDetect(&xKeyCB[i]);

​    }

  }

}
```

2.用于按键检测的定时器应设置为10ms中断一次，用于消抖

3.按键检测示例（以FreeRTOS为例）

```
/**

 * @brief  Task1

 * @param  pvParameters :传入参数

 * @retval  无

 */

void Task1(void *pvParameters)

{

  (void)pvParameters;

  while(1)

  {

​    if(keyGetEvent(KEY0_ID) == KEY_EVENT_PRESS){

​      LED0_TOGGLE();

​    }

​    if(keyGetEvent(KEY1_ID) == KEY_EVENT_PRESS){

​      LED1_TOGGLE();

​    }

​    vTaskDelay(10);

  }

}
```


