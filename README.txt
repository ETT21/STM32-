驱动说明：
该按键驱动实现了硬件层与抽象层分离，按键设备统一管理，可扩展性高，按键控制块实现全局按键消息传递，
事件驱动按键检测，定时器消抖，不阻塞CPU，支持长按、短按，并且避免了短按按键长按误触发。
使用说明：

1.需要再定时器中断回调函数中调用vKeyDetect()用于检测按键

以STM32F103的TIM5为例（假设已经初始化的TIM5）

void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)

{

  if(htim->Instance == TIM5 ){

​    for(uint8_t i = 0; i < KEY_NUM; i++){

​      vKeyDetect(&xKeyCB[i]);

​    }

  }

}

2.用于按键检测的定时器应设置为10ms中断一次，用于消抖

3.按键检测示例（以FreeRTOS为例）

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




