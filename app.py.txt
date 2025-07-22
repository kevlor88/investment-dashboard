import dash
from dash import dcc, html, dash_table
import pandas as pd
import plotly.graph_objects as go

# Sample data for portfolio
portfolio_data = pd.DataFrame({
    'Position': ['NVDA', 'BTC', 'GLD'],
    'Tag': ['Core', 'Core', 'Hedge'],
    'Entry Price': [500, 30000, 180],
    'Current Price': [520, 32000, 182],
    'Performance (%)': [4.0, 6.7, 1.1],
    'Hold Time': ['2 months', '3 months', '1 month']
})

# Sample recommendations
recommendations = pd.DataFrame({
    'Stock/ETF': ['NVDA', 'TSLA', 'GLD'],
    'Tag': ['Core', 'Swing', 'Hedge'],
    'Entry Price': [500, 700, 180],
    'Current Price': [520, 730, 182],
    'Reasoning': ['Breakout semis', 'Earnings beat', 'Macro hedge'],
    'Stop-Loss': [470, 668, 165],
    'Profit Target': [600, 860, 'N/A'],
    'Action': ['Buy', 'Buy', 'Hold']
})

app = dash.Dash(__name__)

app.layout = html.Div([
    html.H1("My Power Market Dashboard", style={'textAlign':'center'}),

    # Market Overview
    html.Div([
        html.H2("Market Overview"),
        html.P("S&P 500: +1.2%"),
        html.P("NASDAQ: +1.5%"),
        html.P("Dow: +0.9%"),
        html.P("Inflation: 3.2%"),
        html.P("Interest Rates: 5.25%"),
        html.P("GDP Growth (QoQ): +2.1%"),
    ], style={'padding': '20px', 'border': '1px solid black'}),

    # Portfolio Snapshot
    html.Div([
        html.H2("Portfolio Snapshot"),
        dash_table.DataTable(
            data=portfolio_data.to_dict('records'),
            columns=[{'name': i, 'id': i} for i in portfolio_data.columns],
            style_table={'overflowX': 'auto'},
        ),
    ], style={'padding': '20px', 'border': '1px solid black'}),

    # Recommendations
    html.Div([
        html.H2("Daily Recommendations"),
        dash_table.DataTable(
            data=recommendations.to_dict('records'),
            columns=[{'name': i, 'id': i} for i in recommendations.columns],
            style_table={'overflowX': 'auto'},
        ),
    ], style={'padding': '20px', 'border': '1px solid black'}),

    # Sample Technical Chart
    html.Div([
        html.H2("Sample Stock Chart"),
        dcc.Graph(
            figure=go.Figure(
                data=[
                    go.Candlestick(
                        x=pd.date_range(start='2024-01-01', periods=5),
                        open=[100, 102, 101, 105, 110],
                        high=[105, 106, 103, 108, 112],
                        low=[99, 100, 99, 104, 109],
                        close=[102, 104, 102, 107, 111]
                    )
                ],
                layout={'title': 'Sample Stock Chart'}
            )
        )
    ], style={'padding': '20px'})
])

if __name__ == '__main__':
    app.run_server(debug=True)
