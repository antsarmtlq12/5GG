import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Upload, BarChart, AlertTriangle, CheckCircle, MessageCircle, Send } from "lucide-react";
import { LineChart, Line, XAxis, YAxis, Tooltip, ResponsiveContainer, BarChart as ReBarChart, Bar, PieChart, Pie, Cell } from "recharts";

export default function NetworkAnalyzer() {
  const [file, setFile] = useState(null);
  const [packetData, setPacketData] = useState([]);
  const [protocolData, setProtocolData] = useState([]);
  const [showChart, setShowChart] = useState(false);
  const [chatMessages, setChatMessages] = useState([
    { sender: "CyberBot", text: "Hello! I am your AI assistant. How can I help you today?" },
  ]);
  const [userMessage, setUserMessage] = useState("");

  const handleFileUpload = (event) => {
    const uploadedFile = event.target.files[0];
    if (uploadedFile) {
      setFile(uploadedFile);
      parseFile();
    }
  };

  const parseFile = () => {
    const mockPacketData = [
      { id: 1, responseTime: 120, transferRate: 5, packetLoss: 2 },
      { id: 2, responseTime: 100, transferRate: 6, packetLoss: 1 },
      { id: 3, responseTime: 150, transferRate: 4, packetLoss: 3 },
    ];
    setPacketData(mockPacketData);

    const mockProtocolData = [
      { name: "TCP", value: 40 },
      { name: "UDP", value: 30 },
      { name: "HTTP", value: 20 },
      { name: "Other", value: 10 },
    ];
    setProtocolData(mockProtocolData);

    setShowChart(true);
  };

  const handleSendMessage = () => {
    if (userMessage.trim()) {
      setChatMessages([...chatMessages, { sender: "User", text: userMessage }]);
      setUserMessage("");
    }
  };

  return (
    <div className="p-6 max-w-4xl mx-auto bg-gray-200 text-black rounded-lg shadow-lg">
      <h1 className="text-3xl font-extrabold text-center mb-6 text-blue-700">5G Network Security Analyzer</h1>

      <div className="mb-4">
        <Input type="file" accept=".pcap" onChange={handleFileUpload} className="mb-2" />
        <Button className="w-full bg-blue-500 text-white flex items-center gap-2"><Upload size={16} /> Upload File</Button>
      </div>

      {showChart && (
        <div>
          <h2 className="text-xl font-bold">Network Data Analysis</h2>
          <ResponsiveContainer width="100%" height={300}>
            <ReBarChart data={packetData}>
              <XAxis dataKey="id" />
              <YAxis />
              <Tooltip />
              <Bar dataKey="responseTime" fill="#8884d8" />
              <Bar dataKey="transferRate" fill="#82ca9d" />
              <Bar dataKey="packetLoss" fill="#ff7300" />
            </ReBarChart>
          </ResponsiveContainer>

          <h2 className="text-xl font-bold mt-6">Protocol Distribution</h2>
          <ResponsiveContainer width="100%" height={300}>
            <PieChart>
              <Pie data={protocolData} dataKey="value" nameKey="name" cx="50%" cy="50%" outerRadius={100} fill="#8884d8" label>
                {protocolData.map((entry, index) => (
                  <Cell key={`cell-${index}`} fill={["#8884d8", "#82ca9d", "#ffc658", "#ff7300"][index % 4]} />
                ))}
              </Pie>
              <Tooltip />
            </PieChart>
          </ResponsiveContainer>

          <h2 className="text-xl font-bold mt-6">Protocol Data Table</h2>
          <table className="w-full bg-white shadow-md rounded-lg overflow-hidden">
            <thead className="bg-gray-300">
              <tr>
                <th className="p-2">ID</th>
                <th className="p-2">Response Time (ms)</th>
                <th className="p-2">Transfer Rate (Mbps)</th>
                <th className="p-2">Packet Loss (%)</th>
              </tr>
            </thead>
            <tbody>
              {packetData.map((data) => (
                <tr key={data.id} className="border-b">
                  <td className="p-2 text-center">{data.id}</td>
                  <td className="p-2 text-center">{data.responseTime}</td>
                  <td className="p-2 text-center">{data.transferRate}</td>
                  <td className="p-2 text-center">{data.packetLoss}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      )}

      <div className="mt-6 p-4 bg-white shadow rounded-lg">
        <h2 className="text-xl font-bold mb-2">Chat with CyberBot</h2>
        <div className="h-40 overflow-auto border p-2 mb-2">
          {chatMessages.map((msg, index) => (
            <p key={index} className={msg.sender === "CyberBot" ? "text-blue-700" : "text-gray-700"}><strong>{msg.sender}:</strong> {msg.text}</p>
          ))}
        </div>
        <div className="flex gap-2">
          <Input value={userMessage} onChange={(e) => setUserMessage(e.target.value)} placeholder="Type a message..." />
          <Button onClick={handleSendMessage} className="bg-green-500 text-white flex items-center gap-2"><Send size={16} /> Send</Button>
        </div>
      </div>
    </div>
  );
}

