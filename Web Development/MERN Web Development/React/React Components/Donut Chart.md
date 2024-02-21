```jsx
import React , {useState} from 'react';
import styled from 'styled-components';
import { rgba } from 'polished';
import {PieChart,Pie} from 'recharts';

const Card = styled.div`
    width: 300px;
    padding: 2rem 0;
    border-radius: 3rem;
    background: #191919;
`;

const ChartWrapper = styled.div`
    position: relative;
    display: flex;
    justify-content: center;
`;

const ChartLabel = styled.div`
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 0.875rem;
    color: ${rgba("white",0.5)};
    opacity: ${({isVisible}) => isVisible ? 1 : 0}};
    text-align: center;
    transition: opacity 0.25s;
`;

const ChartLabelValue = styled.div`
    font-size: 1.75rem;
    color: ${rgba("white",0.85)};
`;

const data = [
    {name : "Lost",value:672 ,fill:"#f31512"},
    {name : "Strong",value:462,fill:"#b239dc"},
    {name : "Moderate",value:127,fill:"#028ffb"},
    {name : "Low",value:41,fill:"#68d42d"}
]

const DonutChartCard = () => {
    const [active, setActive] = useState("");
    const [mouseOver,setMouseOver] = useState(false);
    const activeItem = data.find((obj) => obj.name ===active);
    return (
        <Card>
            <ChartWrapper>
                <ChartLabel isVisible={mouseOver}>
                    {activeItem && activeItem.name}
                    <ChartLabelValue>{active !== "" && activeItem.value}</ChartLabelValue>
                </ChartLabel>
                <PieChart width={210} height={210}>
                    <Pie
                    isAnimated={false}
                    startAngle={90}
                    endAngle={450}
                    strokeWidth={0}
                    data = {data}
                    innerRadius = {60}
                    outerRadius = {80}
                    onMouseEnter={(e) => {
                        setActive(e.name);
                        setMouseOver(true);
                    }}
                    onMouseLeave={(e) => {
                        setMouseOver(false);
                    }}
                    />
                </PieChart>
            </ChartWrapper>
        </Card>
    );
};

export default DonutChartCard;
```