#include "ns3/core-module.h"
#include "ns3/network-module.h"
#include "ns3/internet-module.h"
#include "ns3/csma-module.h"
#include "ns3/point-to-point-module.h"
#include "ns3/netanim-module.h"
using namespace ns3;
int main ()
{
  //  Nodes
  NodeContainer nodes;
  nodes.Create (25);
  InternetStackHelper internet;
  internet.Install (nodes);
  PointToPointHelper p2p;
  p2p.SetDeviceAttribute ("DataRate", StringValue ("10Mbps"));
  p2p.SetChannelAttribute ("Delay", StringValue ("2ms"));
  for (int i = 0; i < 24; i++)
  {
    p2p.Install (nodes.Get(i), nodes.Get(i+1));
  }

  //  BUS (0–4)
  CsmaHelper csma;
  csma.SetChannelAttribute ("DataRate", StringValue ("100Mbps"));
  csma.SetChannelAttribute ("Delay", TimeValue (NanoSeconds (6560)));
  NodeContainer bus;
  for (int i = 0; i <= 4; i++)
    bus.Add (nodes.Get(i));
  csma.Install (bus);
  //  STAR (center = 5, leaves = 6–9)
  for (int i = 6; i <= 9; i++)
    p2p.Install (nodes.Get(5), nodes.Get(i));
  //  RING (10–14)
  for (int i = 10; i < 14; i++)
    p2p.Install (nodes.Get(i), nodes.Get(i+1));
  p2p.Install (nodes.Get(14), nodes.Get(10));
  //  TREE (15–20)
  p2p.Install (nodes.Get(15), nodes.Get(16));
  p2p.Install (nodes.Get(15), nodes.Get(17));
  p2p.Install (nodes.Get(16), nodes.Get(18));
  p2p.Install (nodes.Get(16), nodes.Get(19));
  p2p.Install (nodes.Get(17), nodes.Get(20));
  // MESH (21–24)
  for (int i = 21; i <= 24; i++)
    for (int j = i+1; j <= 24; j++)
      p2p.Install (nodes.Get(i), nodes.Get(j));
  //  NetAnim
  AnimationInterface anim ("five_topologies_25.xml");
  for (int i = 0; i <= 4; i++)
    anim.SetConstantPosition (nodes.Get(i), 50 + i*50, 300);     // Bus

  anim.SetConstantPosition (nodes.Get(5), 300, 450);            // Star center
  for (int i = 6; i <= 9; i++)
    anim.SetConstantPosition (nodes.Get(i), 200 + (i-6)*70, 550);

  for (int i = 10; i <= 14; i++)                                // Ring
    anim.SetConstantPosition (
      nodes.Get(i),
      550 + 80*cos((i-10)*2*M_PI/5),
      350 + 80*sin((i-10)*2*M_PI/5)
    );
  anim.SetConstantPosition (nodes.Get(15), 800, 450);           // Tree
  anim.SetConstantPosition (nodes.Get(16), 750, 550);
  anim.SetConstantPosition (nodes.Get(17), 850, 550);
  anim.SetConstantPosition (nodes.Get(18), 720, 650);
  anim.SetConstantPosition (nodes.Get(19), 780, 650);
  anim.SetConstantPosition (nodes.Get(20), 880, 650);
  anim.SetConstantPosition (nodes.Get(21), 1050, 450);          // Mesh
  anim.SetConstantPosition (nodes.Get(22), 1120, 520);
  anim.SetConstantPosition (nodes.Get(23), 1050, 590);
  anim.SetConstantPosition (nodes.Get(24), 980, 520);
  Simulator::Run ();
  Simulator::Destroy ();
  return 0;
}
