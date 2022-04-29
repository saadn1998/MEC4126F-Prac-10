# MEC4126F-Prac-10

// Description----------------------------------------------------------------|
/*
 * Initialises a struct with Name and Age data. Displays results on LEDs and
 * LCD.
 */
// DEFINES AND INCLUDES-------------------------------------------------------|

#include "stm32f0xx.h"

// GLOBAL VARIABLES ----------------------------------------------------------|


// FUNCTION DECLARATIONS -----------------------------------------------------|

void main(void);
void init_ADC(void);
void init_timer_2(void);
void trigger_ADC_conversion(void);
_Bool ADC_busy(void);
uint16_t read_ADC_value(void);


// MAIN FUNCTION -------------------------------------------------------------|

void main(void)
{


	while(1)
	{

	}
}

// OTHER FUNCTIONS -----------------------------------------------------------|

/* Description:
 * This function initialises the ADC setting to allow 10 bit conversions on PA6
 * in single conversion, wait mode.
 */
void init_ADC(void)
{
	//
	RCC->AHBENR |= RCC_AHBENR_GPIOAEN;
	//
	GPIOA->MODER |= GPIO_MODER_MODER6;
	//
	RCC->APB2ENR |= RCC_APB2ENR_ADCEN;
	//
	ADC1->CR |= ADC_CR_ADEN;
	//
	ADC1->CHSELR |= ADC_CHSELR_CHSEL6;
	// Setup in wait mode
	ADC1->CFGR1 |= ADC_CFGR1_WAIT;
	// Setup 10 bit resolution
	ADC1->CFGR1 |= ADC_CFGR1_RES_0;
	//
	while((ADC1->ISR & ADC_ISR_ADRDY) == 0);
}

/* Description:
 * This function starts the conversion sequence of the ADC
 */
void trigger_ADC_conversion(void)
{
	ADC1->CR |= ADC_CR_ADSTART;
}

/* Description:
 * This function checks whether the ADC conversion sequence is ongoing.
 * It returns true if the conversion is ongoing.
 * It returns false if the conversion is complete and data is ready.
 */
_Bool ADC_busy(void)
{
	return (ADC1->ISR & ADC_ISR_EOC) == 0) ? true : false; // This line uses the ternery operator
}

/* Description:
 * This function reads the numerical result of the ADC conversion.
 * The programmer should ensure that the conversion is complete before reading the result.
 */
uint16_t read_ADC_value(void)
{
	return ADC1->DR;
}

#if 0
void init_timer_2(void)
{
	//clock to the timer
	RCC -> APB1ENR |= RCC_APB1ENR_TIM2EN;
	//Enable clock for PB
	RCC -> AHBENR |= RCC_AHBENR_GPIOBEN;
	//Set PB10 to alternate function
	GPIOB -> MODER |= GPIO_MODER_MODER10_1;
	//Set AFR
	GPIOB -> AFR[1] |= 0x02 << (2*4);

	//Set prescalar to 2
	TIM2 -> PSC = 2;
	//Set ARR to 1023
	TIM2 -> ARR = 1023;
	//Set CCR3 to 1023/4
	TIM2 -> CCR3 = 1023/4; //Using channel 3
	//Select PWM mode 1 on OC3, enable preload register on OC3
	TIM2 -> CCMR3 |= TIM_CCMR2_OC3M_2 | TIM_CCMR2_OC3M_1 | TIM_CCMR2_OC3PE;
	//Select active high polarity on OC3
	TIM2 -> CCER |= TIM_CCER_CC3E
	//Enable counter
	TIM2 -> CR3 |= TIM_CR3_CEN

}
#endif

void init_timer_6(void)
{

}

// INTERRUPT HANDLERS --------------------------------------------------------|

void TIM6_IRQHandler(void)
{
	TIM6->SR &= ~TIM_SR_UIF;	//acknowledge interrupt
}
