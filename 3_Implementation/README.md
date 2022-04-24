# simulation:
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "config.h"
#include <avr/io.h>
#include <util/delay.h>
#include <avr/interrupt.h>
#include "uart_hal.h"
#include "timer1_hal.h"
#include "timer0_hal.h"
int main(void)
{
uart_init(9600,0);
pwm_init();
 sei();
servo_set(0,180);
int16_t i = 0;
 while (1)
 {
for(i = 0; i <= 180; i++){
servo_set(i,180);
_delay_ms(40);
}
for(i = 180; i >= 0; i--){
servo_set(i,180);
_delay_ms(40);
}
 }
}
# UART:
#include "uart_hal.h"
volatile static uint8_t rx_buffer[RX_BUFFER_SIZE] = {0};
volatile static uint16_t rx_count = 0;
volatile static uint8_t uart_tx_busy = 1;
ISR(USART_RX_vect){
volatile static uint16_t rx_write_pos = 0;
rx_buffer[rx_write_pos] = UDR0;
rx_count++;
rx_write_pos++;
if(rx_write_pos >= RX_BUFFER_SIZE){
rx_write_pos = 0;
}
}
ISR(USART_TX_vect){
uart_tx_busy = 1;
}
void uart_init(uint32_t baud,uint8_t high_speed){
uint8_t speed = 16;
if(high_speed != 0){
speed = 8;
UCSR0A |= 1 << U2X0;
}
baud = (F_CPU/(speed*baud)) - 1;
UBRR0H = (baud & 0x0F00) >> 8;
UBRR0L = (baud & 0x00FF);
UCSR0B |= (1 << TXEN0) | (1 << RXEN0) | (1 << TXCIE0) | (1 << RXCIE0);
}
void uart_send_byte(uint8_t c){
while(uart_tx_busy == 0);
uart_tx_busy = 0;
UDR0 = c;
}
int uart_stream_byte(char c, FILE *stream){
if (c == '\n') uart_stream_byte('\r', stream);
while(uart_tx_busy == 0);
uart_tx_busy = 0;
UDR0 = c;
return 0;
}
void uart_send_array(uint8_t *c,uint16_t len){
for(uint16_t i = 0; i < len;i++){
uart_send_byte(c[i]);
}
}
void uart_send_string(uint8_t *c){
uint16_t i = 0;
do{
uart_send_byte(c[i]);
i++;
}while(c[i] != '\0');
uart_send_byte(c[i]);
}
uint16_t uart_read_count(void){
return rx_count;
}
uint8_t uart_read(void){
static uint16_t rx_read_pos = 0;
uint8_t data = 0;
data = rx_buffer[rx_read_pos];
rx_read_pos++;
rx_count--;
if(rx_read_pos >= RX_BUFFER_SIZE){
rx_read_pos = 0;
}
return data;
}
FILE uart_output = FDEV_SETUP_STREAM(uart_stream_byte, NULL, _FDEV_SETUP_WRITE);
void stdio_init(void){
stdout = &uart_output;
}
# TIMER-1:
#include "timer1_hal.h"
volatile static uint8_t update_pwm_ready = 0;
ISR(TIMER1_OVF_vect){
update_pwm_ready = 0;
}
ISR(TIMER1_COMPA_vect){
}
void pwm_init(void){
DDRB |= (1 << DDB1);
TCCR1A = (0b10 << COM1A0) | (0b00 << COM1B0) | (0b10 << WGM10);
TIMSK1 = (1 << TOIE1) | (1 << OCIE1A);
OCR1AH = (SEVRO_MIN & 0xFF00) >> 8;
OCR1AL = (SEVRO_MIN & 0x00FF);
ICR1H = (PWM_TOP & 0xFF00) >> 8;
ICR1L = (PWM_TOP & 0x00FF);
TCCR1B = (0b11 << WGM12) | (0b010 << CS10);
}
static void update_pwm(uint16_t i){
update_pwm_ready = 1;
while(update_pwm_ready != 0);
OCR1AH = (i & 0xFF00) >> 8;
OCR1AL = (i & 0x00FF);
}
void pwm_sweep(void){
uint16_t i = 0;
for(i = SEVRO_MIN; i <= SEVRO_MAX;i=+50){
update_pwm(i);
_delay_ms(40);
}
for(i = SEVRO_MAX; i >= SEVRO_MIN;i-=50){
update_pwm(i);
_delay_ms(40);
}
}
void servo_set(uint16_t deg,uint16_t max_deg){
float set = (float)deg / (float)max_deg;
set = (((float)SEVRO_MAX-(float)SEVRO_MIN)*set) + (float)SEVRO_MIN;
uint16_t piont = (uint16_t)set;
update_pwm(piont);
}
# TIMER-0:
#include "timer0_hal.h"
volatile static uint32_t millis_c = 0;
ISR(TIMER0_COMPA_vect){
millis_c++;
}
void timer0_init(void){
TCCR0A |= (0b10 << WGM00);
OCR0A = 249; // TOP value
TIMSK0 |= (1 << OCIE0A);
TCCR0B |= (0b011<< CS00);
}
uint32_t millis(void){
return millis_c;
}
uint8_t millis_end(uint32_t start_time,uint32_t delay_time){
return ((millis() - start_time) >= delay_time);
}
