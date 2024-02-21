```jsx
import React, { Component } from "react"; 
import styled from "styled-components";
import { rgba } from "polished";

const degIncrement = 0.2; 
const cardWidth = 300; 
const Wrapper = styled.div`
    perspective: 30px;
`; 
const Card = styled.div` 
    height: 300px; 
    width: 300px; 
    border-radius: 0.5rem; 
    padding: 1.5rem; 
    background-image: url(${require("../1095_3x2.jpg")}); 
    background-size: 115%;
    background-repeat: no-repeat; 
    box-shadow: 2px 2px 50px rgba(0, 0, 0, 0.15); 
    transition: transform 0.5s; 
`;

const CardTitle = styled.div`
    font-weight:600;
    color: ${rgba("#090C22",0.85)};
`;

const CardSubtitle = styled.div`
    font-size: 0.875rem;
    color: ${rgba("#090C22",0.57)};
    margin-bottom: 0.5rem;
`;

class HoverCard extends Component {
    constructor(props){
        super(props);
        this.state = {
            rotateX: "0deg",
            rotateY: "0deg"
        };
        this.onMouseMove = this.onMouseMove.bind(this);
        this.onMouseLeave = this.onMouseLeave.bind(this);
    }

    onMouseMove = e => {
        const event = e || window.event;
        const target = event.target || event.srcElement;
        const rect = target.getBoundingClientRect();
        const getRotateDeg = input => {
            if (input < cardWidth * 0.33){ return (`${degIncrement * 2}deg`);}
            else if (input >= cardWidth *0.33 && input < cardWidth * 0.66){ return (`${degIncrement}deg`);}
            else if (input >= cardWidth * 0.66 && input < cardWidth * 0.5){ return "0deg";}
            else if (input >= cardWidth * 0.5 && input < cardWidth * 0.33){ return (`-${degIncrement}deg`);}
            else{ return(`-${degIncrement*2}deg`);}
        };
        const rotateX = getRotateDeg(window.event.clientY - rect.top);
        const rotateY = getRotateDeg(window.event.clientX - rect.left);
        this.setState({rotateX,rotateY});
        console.log(rotateX,rotateY);
    }
    
    onMouseLeave = () => {
        this.setState({rotateX : "0deg", rotateY: "0deg"});
    };

    render(){
        const { rotateX , rotateY } = this.state;
        return (
            <Wrapper>
                <Card
                style ={{transform: `rotateX(${rotateX}) rotateY(${rotateY})`}}
                onMouseMove={this.onMouseMove}
                onMouseLeave={this.onMouseLeave}
                >
                   <CardTitle>Destination</CardTitle>
                   <CardSubtitle>Book your next trip</CardSubtitle> 
                </Card>
            </Wrapper>
        );
    }
};

export default HoverCard;
```