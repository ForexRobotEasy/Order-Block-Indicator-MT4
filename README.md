//+------------------------------------------------------------------+
//|                                                    Order Block Indicator MT4                                                 |
//|                                                      Version 1.0                                                          |
//|                                                                                                                                  |
//|                                        Written by [Your Name]                                                       |
//|                                                                                                                                  |
//|                                        Terms of Reference for Writing Code                                       |
//|                                                                                                                                  |
//|                                        This program is a custom indicator for MT4                          |
//|                                        that identifies and highlights order blocks and supply/demand zones.    |
//|                                                                                                                                  |
//|                                        [Add any additional comments or details here]                        |
//|                                                                                                                                  |
//|                                        Backlink: [https://forexroboteasy.com/forex-robot-review/review-order-block-indicator-mt4s-precision-in-forex-trading/] |
//+------------------------------------------------------------------+

#property strict

// Input parameters
input int blockSize = 5;  // Size of the order blocks
input double sensitivity = 0.5;  // Sensitivity of the indicator
input color blockColor = clrYellow;  // Color of the order blocks

// Global variables
double orderBlocksBuffer[];
double supplyDemandZonesBuffer[];

//+------------------------------------------------------------------+
//| Custom indicator initialization function                         |
//+------------------------------------------------------------------+
int OnInit()
{
   IndicatorBuffers(2);
   
   // Set buffer for order blocks
   SetIndexBuffer(0, orderBlocksBuffer);
   SetIndexStyle(0, DRAW_ARROW);
   SetIndexArrow(0, SYMBOL_ARROWUP);
   SetIndexEmptyValue(0, EMPTY_VALUE);
   
   // Set buffer for supply/demand zones
   SetIndexBuffer(1, supplyDemandZonesBuffer);
   SetIndexStyle(1, DRAW_COLOR_SECTION);
   SetIndexEmptyValue(1, EMPTY_VALUE);
   
   return(INIT_SUCCEEDED);
}

//+------------------------------------------------------------------+
//| Custom indicator deinitialization function                       |
//+------------------------------------------------------------------+
void OnDeinit(const int reason)
{
   // Clean up buffers
   ArrayInitialize(orderBlocksBuffer, EMPTY_VALUE);
   ArrayInitialize(supplyDemandZonesBuffer, EMPTY_VALUE);
}

//+------------------------------------------------------------------+
//| Custom indicator calculation function                            |
//+------------------------------------------------------------------+
int OnCalculate(const int rates_total,
                const int prev_calculated,
                const datetime &time[],
                const double &open[],
                const double &high[],
                const double &low[],
                const double &close[],
                const long &tick_volume[],
                const long &volume[],
                const int &spread[])
{
   // Calculate order blocks and supply/demand zones based on the input parameters
   
   // [Add your code here to identify order blocks and supply/demand zones]
   
   // Plot order blocks
   for (int i = 0; i < ArraySize(orderBlocksBuffer); i++)
   {
      if (orderBlocksBuffer[i] != EMPTY_VALUE)
      {
         double blockPrice = orderBlocksBuffer[i];
         string blockLabel = 'Order Block';
         
         if (blockPrice > high[i])
            blockLabel += ' (Sell)';
         else if (blockPrice < low[i])
            blockLabel += ' (Buy)';
         
         ChartSetInteger(0, CHART_COMMENT, i, blockLabel);
         ChartArrow(0, time[i], blockPrice);
      }
   }
   
   // Plot supply/demand zones
   for (int i = 0; i < ArraySize(supplyDemandZonesBuffer); i += 2)
   {
      if (supplyDemandZonesBuffer[i] != EMPTY_VALUE && supplyDemandZonesBuffer[i + 1] != EMPTY_VALUE)
      {
         double zoneStart = supplyDemandZonesBuffer[i];
         double zoneEnd = supplyDemandZonesBuffer[i + 1];
         
         ChartSetInteger(0, CHART_COMMENT, i, 'Supply/Demand Zone');
         ChartSetInteger(0, CHART_COMMENT, i + 1, 'Supply/Demand Zone');
         ChartSetInteger(0, CHART_COLOR, i, blockColor);
         ChartSetInteger(0, CHART_COLOR, i + 1, blockColor);
         
         ChartRectangle(0, time[i], zoneStart, time[i], zoneEnd);
      }
   }
   
   return(rates_total);
}
